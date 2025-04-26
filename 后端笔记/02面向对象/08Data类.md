<center>Date类</center>







[toc]









## Date类

> Date类：



```php
<?php

namespace utils;

class Date
{

    public static string $format = 'Y-m-d H:i:s';
    const FORMAT_DAY = 'Y-m-d';
    const FORMAT_MONTH = 'Y-m';
    const FORMAT_SECOND = 'Y-m-d H:i:s';
    const START_SECOND = '00:00:00';
    const END_SECOND = '23:59:59';
    const CALCULATE_MODE_SECOND = 1;
    const CALCULATE_MODE_MIN = 2;
    const CALCULATE_MODE_HOUR = 3;
    const CALCULATE_MODE_DAY = 4;
    private static string $start = '00:00:00';
    private static string $end = '23:59:59';

    const DIVISOR = [
        self::CALCULATE_MODE_SECOND => 1,
        self::CALCULATE_MODE_MIN => 60,
        self::CALCULATE_MODE_HOUR => 3600,
        self::CALCULATE_MODE_DAY => 86400
    ];

    /**
     * 格式化
     * @param int $time
     * @return string
     */
    public static function format(int $time, $format = self::FORMAT_SECOND): string
    {
        return date($format, $time);
    }

    /**
     * @param string $format
     * @return string
     */
    public static function now(string $format = ''): string
    {
        return date($format ?: self::$format);
    }

    public static function week(string $date){
        return date('w', strtotime($date));
    }

    public static function nowMonth()
    {
        return date('Y-m');
    }

    public static function month($date)
    {
        return date('Y-m', strtotime($date));
    }

    /**
     * 从指定日减去$subDay天
     * @param string $date
     * @param int $subDay
     * @param string $format
     * @return string
     */
    public static function subDay(string $date, int $subDay, string $format = ''): string
    {
        $dateTime = date_create($date);
        $subDayStr = $subDay . " days";
        $res = date_sub($dateTime, date_interval_create_from_date_string($subDayStr));
        return date_format($res, $format ?: self::FORMAT_DAY);
    }

    public static function addDay($date, $addDay, $format  = ''){
        $dateTime =  date_create($date);
        $addDayStr  =  $addDay.' days';
        $res = date_add($dateTime, date_interval_create_from_date_string($addDayStr));
        return date_format($res, $format ?: self::FORMAT_DAY);
    }

    /**
     * 求两个日期相差天数
     * @param string $date1
     * @param string $date2
     * @return int
     */
    public static function dateSubDays(string $date1, string $date2): int
    {
        $end = strtotime($date1);
        $start = strtotime($date2);
        return (int)round(abs(bcsub($end, $start)) / 86400);
    }

    /**
     * 从指定月减去$subMonth月
     * @param string $date
     * @param int $subMonth
     * @param string $format
     * @return string
     */
    public static function subMonth(string $date, int $subMonth, string $format = ''): string
    {
        empty($format) && $format = 'Y-m';
        $dateTime = date_create($date);
        $subStr = $subMonth . " months";
        $res = date_sub($dateTime, date_interval_create_from_date_string($subStr));
        return date_format($res, $format);
    }


    /**
     * 加天数
     * @param string $date
     * @param int $day
     * @param string $format
     * @return string
     */
    public static function plusDays(string $date, int $day, string $format = ''): string
    {
        $time = strtotime($date) + 86400 * $day;
        return date($format ?: self::$format, $time);
    }

    /**
     * 减天数
     * @param string $date
     * @param int $day
     * @param string $format
     * @return string
     */
    public static function subDays(string $date, int $day, string $format = ''): string
    {
        $time = strtotime($date) - 86400 * $day;
        return date($format ?: self::$format, $time);
    }

    /**
     * 日期格式化为月
     * @param $date
     * @return string
     */
    public static function dateToMonth($date): string
    {
        return date('Y-m', strtotime($date));
    }

    /**
     * 日期当天开始时间
     * @param $date 不传默认当天
     * @return string
     */
    public static function startOfDate($date = ''): string
    {
        empty($date) && $date = self::now();
        return date(self::FORMAT_DAY, strtotime($date)) . ' ' . self::START_SECOND;
    }

    /**
     * 日期当天结束时间
     * @param $date 不传默认当天
     * @return string
     */
    public static function endOfDate($date = ''): string
    {
        empty($date) && $date = self::now();
        return date(self::FORMAT_DAY, strtotime($date)) . ' ' . self::END_SECOND;
    }

    /**
     * 今日开始时间
     * @return string
     */
    public static function startOfToday(): string
    {
        return date(self::FORMAT_DAY) . ' ' . self::START_SECOND;
    }

    /**
     * 今日结束时间
     * @return string
     */
    public static function endOfToday(): string
    {
        return date(self::FORMAT_DAY) . ' ' . self::END_SECOND;
    }

    /**
     * $source是否在$target之后
     * @param $source
     * @param string $target
     * @param bool $eqSupport 是否允许边界相等
     * @return bool
     */
    public static function after($source, string $target = '', bool $eqSupport = false): bool
    {
        empty($target) && $target = self::now();
        return $eqSupport ? strtotime($source) >= strtotime($target) : strtotime($source) > strtotime($target);
    }

    /**
     * $source是否在$target之前
     * @param $source
     * @param string $target
     * @param bool $eqSupport 是否允许边界相等
     * @return bool
     */
    public static function before($source, string $target = '', bool $eqSupport = false): bool
    {
        empty($target) && $target = self::now();
        return $eqSupport ? strtotime($source) <= strtotime($target) : strtotime($source) < strtotime($target);
    }

    /**
     * 条件查询
     * @param $type 1:今日  2:昨日  3:近七天  4:近30天
     * @return array
     */
    public static function getBetweenDate($type)
    {
        switch ($type) {
            case 1:
                $res = self::betweenToday();
                break;
            case 2:
                $res = self::betweenYesterday();
                break;
            case 3:
                $res = self::betweenRecentWeek();
                break;
            case 4:
                $res = self::betweenRecentMonth();
                break;
            default:
                $res = [];
        }
        return $res;
    }

    /**
     * 某月开始时间  默认当月
     * @param string $month
     * @return string
     */
    public static function startOfMonth(string $month = ''): string
    {
        empty($month) && $month = date(self::FORMAT_MONTH);
        return $month . '-01 ' . self::START_SECOND;
    }

    /**
     * 某月结束时间 默认当月
     * @param string $month
     * @return string
     */
    public static function endOfMonth(string $month = ''): string
    {
        empty($month) && $month = date(self::FORMAT_MONTH);
        return $month . '-' . date('t') . ' ' . self::END_SECOND;
    }


    /**
     * 今日时间范围
     * @return string[]
     */
    public static function betweenToday(): array
    {
        return [
            self::now('Y-m-d') . " " . self::$start,
            self::now('Y-m-d') . ' ' . self::$end
        ];
    }

    /**
     * 昨天时间范围
     * @return string[]
     */
    public static function betweenYesterday(): array
    {
        $yesterday = self::subDay(self::now(), 1, self::FORMAT_DAY);
        return [
            $yesterday . ' ' . self::START_SECOND,
            $yesterday . ' ' . self::END_SECOND
        ];
    }


    /**
     * 近七天
     * @return string[]
     */
    public static function betweenRecentWeek(): array
    {
        $end = self::now(self::FORMAT_DAY) . ' ' . self::END_SECOND;
        $start = self::subDay($end, 7) . ' ' . self::START_SECOND;
        return [
            $start,
            $end
        ];
    }

    /**
     * 近三十天时间范围
     * @return string[]
     */
    public static function betweenRecentMonth(): array
    {
        $endDay = self::now(self::FORMAT_DAY);
        $startDay = self::subDay($endDay, 30);
        return [
            $startDay . ' ' . self::START_SECOND,
            $endDay . ' ' . self::END_SECOND
        ];
    }

    /**
     * 判断是否在今天
     * @param $date
     * @return bool
     */
    public static function inToday($date): bool
    {
        return self::after($date, self::startOfToday()) && self::before($date, self::endOfToday());
    }

    /**
     * 判断是否在本月
     * @param $date
     * @return bool
     */
    public static function inThisMonth($date): bool
    {
        return self::after($date, self::startOfMonth()) && self::before($date, self::endOfMonth());
    }

    /**
     * 计算两个日期相差时间
     * @param $date1
     * @param $date2
     * @param int $mode 1:秒数  2:分钟数  3:小时  4:天数
     * @return float
     */
    public static function sub($date1, $date2, int $mode = self::CALCULATE_MODE_MIN)
    {
        if (empty($date1) || empty($date2)) {
            return 0;
        }
        $time1 = strtotime($date1);
        $time2 = strtotime($date2);
        $abs = abs(bcsub($time1, $time2));
        $divisor = self::DIVISOR[$mode] ?? 1;
        return ceil(bcdiv($abs, $divisor));
    }

    /**
     * 当前时间减去指的分钟数
     * @param $date
     * @param $minutes
     * @return false|string
     */
    public static function subMinutes($date, $minutes){
        $time = strtotime($date);
        $resTime = $time - $minutes*60;
        return date(self::FORMAT_SECOND, $resTime);
    }

    /**
     * 判断日期是否在指定日期时间内
     * @param $date
     * @param array $dates
     * @param bool $eqSupport 支持相等的边界
     * @return bool
     */
    public static function between($date, array $dates, bool $eqSupport = false): bool
    {
        return self::after($date, $dates[0], $eqSupport) && self::before($date, $dates[1], $eqSupport);
    }

    /**
     * 判断日期是否在最近一个月内
     * @param $date
     * @return bool
     */
    public static function inRecentMonth($date): bool
    {
        $dates = self::betweenRecentMonth();
        return self::between($date, $dates);
    }

    /**
     * 判断两个时间段是否存在交叉
     * @param $dateLine1
     * @param $dateLine2
     * @return bool
     */
    public static function hasCross($dateLine1, $dateLine2): bool
    {
        if ($dateLine1[0] == $dateLine2[0] && $dateLine1[1] == $dateLine2[1]){
            return true;
        }
        foreach ($dateLine1 as $date){
            if (self::between($date, $dateLine2)){
                return true;
            }
        }
        return false;
    }

}
```

