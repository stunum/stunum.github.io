---
layout: post
title: wordpress的灵巧改动
date: 2016-05-23 14:25:16
tags:  水滴石穿
---

### 一、Wordpress[分类目录]小工具显示没有文章的分类
只需要将下列代码添加到该主题的functions.php文件即可：

```
add_filter( 'widget_categories_args', 'wpdx_show_empty_cats' );
function wpdx_show_empty_cats($cat_args) {
    $cat_args['hide_empty'] = 0;
    return $cat_args;
}
```
### 二、让WordPress支持使用中文用户名注册和登录
只需要将下列代码添加到该主题的functions.php文件即可：
```
function ludou_sanitize_user ($username, $raw_username, $strict) {
  $username = wp_strip_all_tags( $raw_username );
  $username = remove_accents( $username );
  // Kill octets
  $username = preg_replace( '|%([a-fA-F0-9][a-fA-F0-9])|', '', $username );
  $username = preg_replace( '/&.+?;/', '', $username ); // Kill entities
  // 网上很多教程都是直接将$strict赋值false，
  // 这样会绕过字符串检查，留下隐患
  if ($strict) {
    $username = preg_replace ('|[^a-z\p{Han}0-9 _.\-@]|iu', '', $username);
  }
  $username = trim( $username );
  // Consolidate contiguous whitespace
  $username = preg_replace( '|\s+|', ' ', $username );
  return $username;
}
add_filter ('sanitize_user', 'ludou_sanitize_user', 10, 3);
```
### 三、显示为比较科学的几天前格式。
代码可以让 30 天内发布的文章显示为几天前，而过了 30 天即显示为正常的标准格式日期。
只需要将下列代码添加到该主题的functions.php文件即可：
```
function Bing_filter_time(){
        global $post ;
        $to = time();
        $from = get_the_time('U') ;
        $diff = (int) abs($to - $from);
        if ($diff <= 3600) {
                $mins = round($diff / 60);
                if ($mins <= 1) {
                        $mins = 1;
                }
                $time = sprintf(_n('%s 分钟', '%s 分钟', $mins), $mins) . __( '前' , 'Bing' );
        }
        else if (($diff <= 86400) && ($diff > 3600)) {
                $hours = round($diff / 3600);
                if ($hours <= 1) {
                        $hours = 1;
                }
                $time = sprintf(_n('%s 小时', '%s 小时', $hours), $hours) . __( '前' , 'Bing' );
        }
        elseif ($diff >= 86400) {
                $days = round($diff / 86400);
                if ($days <= 1) {
                        $days = 1;
                        $time = sprintf(_n('%s 天', '%s 天', $days), $days) . __( '前' , 'Bing' );
                }
                elseif( $days > 29){
                        $time = get_the_time(get_option('date_format'));
                }
                else{
                        $time = sprintf(_n('%s 天', '%s 天', $days), $days) . __( '前' , 'Bing' );
                }
        }
        return $time;
}
add_filter('the_time','Bing_filter_time');
```
### 四、使用第三方主题时显示 WordPress 后台添加的ICP备案号
由于后台添加的ICP备案号只对Wordpress自带的主题生效，所以我们可以在footer.php中添加下列代码让第三方主题支持显示备案号。
```
<a href="http://www.miitbeian.gov.cn/" rel="external nofollow" target="_blank">
<?php echo get_option( 'zh_cn_l10n_icp_num' );?>
</a>
```
如果不愿意链接到工信部网站，则只需添加以下代码即可。
```
<?php echo get_option( 'zh_cn_l10n_icp_num' );?>
```
### 五、WORDPRESS 新用户注册邮件链接无效的解决办法
情况描述：由于邮箱收到密码页面的链接两端有<......>导致链接无法使用
步骤：

1）找到wp-includes文件夹中的pluggable.php文件并打开（不要用记事本打开）

2）找到语句： 
```
$message .= '<'network_site_url("wp-login.php?action=rp&key=$key&login=" . rawurlencode($user_login), 'login')">\r\n\" ;
```
3）替换为：
```
$message .= '<' . network_site_url("wp-login.php?action=rp&key=$key&login=" . rawurlencode($user->user_login), 'login') . "\r\n\r\n" ;
```
保存并将修改过的pluggable.php文件上传至网站根目录覆盖即可。