# NGINX-based Media Streaming Server
## nginx-rtmp-module
this copy is trying to fix a bug happening during notify procedure.

## problem
if you put a conf relate to notify module

for example:
  on_update http://www.abc.com/onupdate;

the ip for www.abc.com will be acquried at the beginning when you start the nginx.
the trick thing is. if the ip address you get is no longer valid for www.abc.com after couple of days
all the notify procedure will faild. 

ok. 
restart the nginx sever will fix this problem.


## fixed in code

A function has been added to ngx_rtmp_notify_module.c 
ngx_rtmp_notify_parse_urlstr(ngx_url_t *u, char *url, ngx_rtmp_session_t *s)

this function is going to refresh the ip for given url.

###this function will be called in side of given functions below

functions                     |    event in config
-|-
ngx_rtmp_notify_update_handle  |   on_update
ngx_rtmp_notify_connect        |   on_connect
ngx_rtmp_notify_disconnect|
ngx_rtmp_notify_publish        |   on_publish
ngx_rtmp_notify_play          |    on_play
ngx_rtmp_notify_done          |    on_done


All those change is fit for my business, so i didn't refresh ip for all event.

