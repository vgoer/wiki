<center>ui-slint</center>





[toc]







## slint

> Slint 曾经被称为 SixtyFPS，特点是既快又小，是一个 Rust 编写的综合性 UI 工具包，**用于为桌面和嵌入式设备构建原生用户界面**





### 1. 创建和安装依赖

> 添加

```shell
cargo add slint@1.0.0

# 直接添加
[dependencies]
slint = "1.0.0"
```

> 窗口： 

````rust
fn main() {
    // 运行slintUI窗体
    MainWindow::new().unwrap().run().unwrap();
}
// slint宏，创建 UI
slint::slint! {
    export component MainWindow inherits Window {
        title: "Main Window";
        width: 600px;
        height: 500px;
        // 定义一个 Text 组件
        Text{
            text: "Hello, world";
            color:blue;
        }
    }
}
````





### 2. 计算器

```rust
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
   // 创建窗体应用程序
   let app =  App::new().unwrap();
   let weak = app.as_weak();// as_weak避免内存泄露
   let state: Rc<RefCell<CalcState>> = Rc::new(RefCell::new(CalcState::default()));
   // 处理.slint中 CalcLogic 全局单例的回调
   app.global::<CalcLogic>().on_button_pressed(move |value| {
      let app = weak.unwrap();
      let mut state = state.borrow_mut();
      println!("pressed value: {}", value);
      // 只处理输入的数字，保存到 state 中
      if let Ok(val) = value.parse::<i32>() {
        state.current_value *=10;
        state.current_value += val;
        app.set_value(state.current_value);
        return;
      }
      // 处理等号"="逻辑
      if value.as_str() == "=" {
        let reslut = match state.operator.as_str() {
            "+" => state.prev_value + state.current_value,
            "-" => state.prev_value - state.current_value,
            "*" => state.prev_value * state.current_value,
            "/" => state.prev_value / state.current_value,
            _=> state.current_value,
        };
        // 输出结果
        app.set_value(reslut);
        println!("{} {} {} = {}", state.prev_value, state.operator, state.current_value, reslut);
        // 重置 state
        state.current_value = 0;
        state.prev_value = reslut;
        state.operator = Default::default();
      } else {
        state.operator = value.clone();
        state.prev_value = state.current_value;
        state.current_value = 0;
      }
   });
   // 运行窗体程序
   app.run().unwrap();
   println!("Hello, world! Hello, Slint!");

}

// 保存输入数据的结构体
#[derive(Default)]
struct CalcState {
   prev_value: i32,
   current_value: i32,
   operator:slint::SharedString,
}

// Slint 宏构建 UI
slint::slint! {
    import { VerticalBox } from "std-widgets.slint";

    // 导出全局单例：Rust 代码可以操作
    export global CalcLogic {
        callback button-pressed(string);
    }
    // 自定义 Button 组件
    component Button {
        in property <string> text;
        min-height:30px;
        min-width: 30px;
        in property <brush> background: @linear-gradient(-20deg, #a0a3e4, #3c58e3);
        Rectangle {
            background: ta.pressed ? red :ta.has-hover? background.darker(10%) : background;
            animate background {duration: 100ms; }
            border-radius: 4px;
            border-width: 2px;
            border-color: self.background.darker(20%);
            ta := TouchArea {
                // Button初始化
                init => { debug("Button init"); }
                // Button点击事件，回传给 Rust 处理
                clicked => {
                    debug("Button clicked");
                    CalcLogic.button-pressed(root.text)
                }
            }
            Text {text: root.text;}
        }
    }
    export component App inherits Window {
        title: "Slint Calculator";
        in property <int> value: 0 ;
        // Slint 内嵌的网格组件
        GridLayout {
            padding: 10px;
            spacing: 5px;
            Text {text: value; colspan: 3;}
            Row {
                Button {text: "1";}
                Button {text: "2";}
                Button {text: "3";}
                Button {text: "+"; background: yellow;}
            }
            Row {
                Button {text: "4";}
                Button {text: "5";}
                Button {text: "6";}
                Button {text: "-"; background: yellow;}
            }
            Row {
                Button {text: "7";}
                Button {text: "8";}
                Button {text: "9";}
                Button {text: "*"; background: yellow;}
            }
            Row {
                Button {text: "0";}
                Button {text: "="; col: 2; background: green;}
                Button {text: "/"; background: yellow;}
            }
        }
    }
}
```



### 3. 拼图

> 国旗： [flag](https://github.com/yammadev/flag-icons)

```rust
# 添加依赖
rand = "0.8"
slint = "1.0.0"
```

> `mian.rc` 

````rust
fn main() {
    use slint::Model;

    let main_window = MainWindow::new().unwrap();

    // Fetch the tiles from the model(从 Model中获取图块列表)
    let mut tiles: Vec<TileData> = main_window.get_memory_tiles().iter().collect();
    // 复制一份图块数据，确保整体上是成对的，因为游戏就要上翻牌来配对图块
    tiles.extend(tiles.clone());

    // 随机混合一下，就是打乱所有图标
    use rand::seq::SliceRandom;
    let mut rng = rand::thread_rng();
    tiles.shuffle(&mut rng);

    // 将打乱后的 Vec 分配给模型属性
    let tiles_model = std::rc::Rc::new(slint::VecModel::from(tiles));
    main_window.set_memory_tiles(tiles_model.clone().into());

    // 游戏规则：游戏规则应强制要求最多两块牌的图块打开，
    // 如果图块匹配，那么我们认为它们已解决并且它们保持打开状态。
    // 否则等一会儿，这样玩家就可以记住图标的位置，然后再次关闭它们。
    let main_window_weak = main_window.as_weak();
    // 在 Rust 方面，我们现在可以向回调添加一个处理程序check_if_pair_solved，它将检查是否打开了两个图块
    main_window.on_check_if_pair_solved(move || {
        let mut flipped_tiles =
            tiles_model.iter().enumerate().filter(|(_, tile)| tile.image_visible && !tile.solved);

        if let (Some((t1_idx, mut t1)), Some((t2_idx, mut t2))) =
            (flipped_tiles.next(), flipped_tiles.next())
        {
            let is_pair_solved = t1 == t2;
            if is_pair_solved { // 如果它们2个匹配，则该solved属性在模型中设置为 true
                t1.solved = true; 
                tiles_model.set_row_data(t1_idx, t1);
                t2.solved = true;
                tiles_model.set_row_data(t2_idx, t2);
            } else {    
                // 如果它们不匹配，则启动一个计时器，该计时器将在一秒钟后关闭它们。当计时器运行时，禁用每个图块，因此在此期间不能单击任何内容。
                let main_window = main_window_weak.unwrap();
                main_window.set_disable_tiles(true); // 禁止单击

                let tiles_model = tiles_model.clone();
                slint::Timer::single_shot(std::time::Duration::from_secs(1), move || {
                    main_window.set_disable_tiles(false);
                    t1.image_visible = false;
                    tiles_model.set_row_data(t1_idx, t1);
                    t2.image_visible = false;
                    tiles_model.set_row_data(t2_idx, t2);
                });
            }
        }
    });

    main_window.run().unwrap();

}
// slint！宏构建 slintUI
slint::slint! {
    // Added: tile 数据结构定义并将其粘贴到slint!宏内部的顶部：
    struct TileData {
        image: image,
        image_visible: bool,
        solved: bool,
    }
    component MemoryTile inherits Rectangle {
        callback clicked;
        in property <bool> open_curtain;
        in property <bool> solved;
        in property <image> icon;
    
        height: 64px;
        width: 64px;
        background: solved ? #34CE57 : #3960D5;
        animate background { duration: 800ms; }
    
        Image {
            source: icon;
            width: parent.width;
            height: parent.height;
        }
        // 分成左右两半的原因是点击后图块的关闭动画
        // Left curtain
        Rectangle {
            background: #193076;
            x: 0px;
            width: open_curtain ? 0px : (parent.width / 2);
            height: parent.height;
            animate width { duration: 250ms; easing: ease-in; }
        }
    
        // Right curtain
        Rectangle {
            background: #193076;
            x: open_curtain ? parent.width : (parent.width / 2);
            width: open_curtain ? 0px : (parent.width / 2);
            height: parent.height;
            animate width { duration: 250ms; easing: ease-in; }
            animate x { duration: 250ms; easing: ease-in; }
        }
    
        TouchArea {
            clicked => {
                // Delegate to the user of this element
                root.clicked();
            }
        }
    }
    
    export component MainWindow inherits Window {
        width: 326px;
        height: 326px;

        callback check_if_pair_solved(); // Added 回调函数：检查两个图块是否打开已配对
        in property <bool> disable_tiles; // Added
 
        in property <[TileData]> memory_tiles: [
            { image: @image-url("icons/CN.png") },
            { image: @image-url("icons/US.png") },
            { image: @image-url("icons/ST.png") },
            { image: @image-url("icons/PE.png") },
            { image: @image-url("icons/MY.png") },
            { image: @image-url("icons/KN.png") },
            { image: @image-url("icons/GY.png") },
            { image: @image-url("icons/FJ.png") },

        ];
        // 该for tile[i] in memory_tiles:语法声明了一个变量 tile，
        // 其中包含数组中一个元素的数据memory_tiles，以及一个变量i，该变量是图块的索引。
        // 我们使用i索引根据其行和列计算图块的位置，使用模和整数除法创建 4 x 4 网格。
        // 运行它会为我们提供一个显示 8 个图块的窗口，这些图块可以单独打开。
        for tile[i] in memory_tiles : MemoryTile {
            x: mod(i, 4) * 74px + 20px; // 调整坐标，尽量使整体居中
            y: floor(i / 4) * 74px + 20px; // 调整坐标，尽量使整体居中
            width: 64px;
            height: 64px;
            icon: tile.image;
            open_curtain: tile.image_visible || tile.solved;
            // propagate the solved status from the model to the tile
            solved: tile.solved;
            clicked => {
                // old: tile.image_visible = !tile.image_visible;
                // new: 当计时器运行时，禁用每个图块，因此在此期间不能单击任何内容。
                if (!root.disable_tiles) { 
                    tile.image_visible = !tile.image_visible;
                    root.check_if_pair_solved();
                }
            }
        }
    }
}

````





















