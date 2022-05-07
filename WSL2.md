### bashrc

```bash
##### proxy list #####
# https://zhuanlan.zhihu.com/p/47849525
# https://zhuanlan.zhihu.com/p/153124468
# 注意不 export 的话，别的 bash 脚本是访问不到这个变量的，为了能在 .ssh/config 访问到，必须 export 一下
# https://unix.stackexchange.com/a/495163
export host_ip=$(cat /etc/resolv.conf |grep "nameserver" |cut -f 2 -d " ")
# wget 比较特殊，不认 all_proxy，只认 http_proxy 和 https_proxy
alias proxy="export all_proxy=http://$host_ip:7890 http_proxy=http://$host_ip:7890 https_proxy=http://$host_ip:7890"
alias unproxy='unset all_proxy http_proxy https_proxy'
proxy
```
