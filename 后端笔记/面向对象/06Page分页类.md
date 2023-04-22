---
title: 06.Page分页类
description: Page分页类
published: 1
date: 2023-04-22T18:10:59.957Z
tags: mysql, oop, php
editor: markdown
dateCreated: 2023-04-22T18:10:59.957Z
---

<center>page</center>

[toc]

## page

> 分页类

```php
namespace Page;
class page{
    private $current; // 当前页的参数
    private $count;// 总资源(查询数据表的总数据)
    private $limit; // 查询出每一页有多少条数据
    private $size; // 页码
    private $class; // 分页样式

    public function __construct($current,$count,$limit,$size,$class="digg")
    {
        $this->current = $current;
        $this->count = $count;
        $this->limit = $limit;
        $this->size = $size;
        $this->class = $class;
    }

    public function get_url(){
        $str = $_SERVER['PHP_SELF'].'?';
        if(!empty($_GET)){
            foreach($_GET as $key=>$v){//不是分页参数，其他参数给保留下来
                if($key != 'page'){
                    // index.php?id=1&page=2
                    $str.= "$key=$v&";
                }

            }
        }
        return $str;
    }
    // 生成分页方法
    public function page(){
        $str = '';
        if($this->count > $this->limit){
            $url = $this->get_url;
            $pages = ceil($this->count / $this->limit);// 共有多少页
            $str.= '<div class="'.$this->class.'">';
            // 第一种情况，只有首页和上一页
            if($this->current != 1){
                /* 
                        如果当前页是1时，它就不会有首页和上一页
                        [1] 2 3 4
                        如果当前页是2时，那么它就会有首页和上一页
                        1 [2] 3 4
                    */
                $str.= '<a href="'. $url .'page=1">首页</a>';
                $str.= '<a href="'. $url .'page='.($this->current-1).'">上一页</a>';
            }

            /* 
                    如果显示页码只有5页
                    1 2 3 4 5
                    当前页在显示的页码的左侧
                    [1] 2 3 4 5
                    1 [2] 3 4 5

                    当前页在显示的页码的中间
                    1 2 [3] 4 5

                    当前页在显示的页码的右侧 一般当前页在显示的页码右侧，说明分页差不多没了
                    1 2 3 [4] 5
                    1 2 3 4 [5]
                */

            if($this->current < ceil($this->size / 2)){
                $start = 1; // 开始的位置
                $end = $pages < $this->size ? $pages : $this->size; // 
            }elseif ($this->current >$pages - floor($this->size / 2)) {
                $start = $pages-$this->size+1 <=0 ? 1:$pages-$this->size+1;
                $end = $pages;
            }else {
                $start = $this->current - floor($this->size / 2);
                $end = $this->current + floor($this->size / 2);
            }

            for($i=$start;$i<=$end;$i++){
                if($i == $this->current){
                    $str.='<span class="current">'.$i.'</span>';
                }else{
                    $str.='<a href="'. $url .'page='.$i.'">'.$i.'</a>';
                }
            }
            // 第三种情况，只有尾页和下一页
            if($this->current != $pages){
                $str.= '<a href="'. $url .'page='.($this->current+1).'">下一页</a>';
                $str.= '<a href="'. $url .'page='.$pages.'">尾页</a>';
            }
            $str.='</div>';
        }
        return $str;
    }
}
```



