# WSL下使用GUI应用
> 参考：
>
> [IDEA中文变方块问题](https://monkeywie.cn/2021/09/26/wsl2-gui-idea-config/#:~:text=%E5%AE%89%E8%A3%85%E5%A5%BD%20IDEA%20%E4%B9%8B%E5%90%8E%EF%BC%8C%E6%89%93%E5%BC%80%E5%8F%91%E7%8E%B0%E4%B8%AD%E6%96%87%E5%85%A8%E9%83%A8%E5%8F%98%E6%88%90%E4%BA%86%E6%96%B9%E5%9D%97%E4%B9%B1%E7%A0%81%EF%BC%8C%E4%B8%BA%E4%BA%86%E8%A7%A3%E5%86%B3%E8%BF%99%E4%B8%AA%E9%97%AE%E9%A2%98%E9%9C%80%E8%A6%81%E5%AE%89%E8%A3%85%E4%B8%AD%E6%96%87%E5%AD%97%E4%BD%93%E3%80%82%20%E5%88%9B%E5%BB%BA%20%2Fetc%2Ffonts%2Flocal.conf%20%E6%96%87%E4%BB%B6%EF%BC%8C%E5%86%85%E5%AE%B9%E5%A6%82%E4%B8%8B%EF%BC%9A%20%E7%94%B1%E4%BA%8E,wsl%20gui%20%E5%BA%95%E5%B1%82%E4%BE%9D%E6%97%A7%E6%98%AF%E5%9F%BA%E4%BA%8E%20RDP%20%E7%9A%84%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E5%AE%9E%E7%8E%B0%EF%BC%8C%E5%AE%98%E6%96%B9%E6%9E%B6%E6%9E%84%E5%9B%BE%EF%BC%9A%20%E6%89%80%E4%BB%A5%E8%BE%93%E5%85%A5%E6%B3%95%E4%B8%8D%E8%83%BD%E4%BD%BF%E7%94%A8%20Windows%20%E5%AE%BF%E4%B8%BB%E6%9C%BA%E4%B8%8A%E7%9A%84%EF%BC%8C%E9%9C%80%E8%A6%81%E5%9C%A8%E8%99%9A%E6%8B%9F%E6%9C%BA%E9%87%8C%E5%AE%89%E8%A3%85%E8%BE%93%E5%85%A5%E6%B3%95%EF%BC%8C%E7%9B%AE%E5%89%8D%E6%9C%89%E4%B8%80%E4%B8%AA%E5%BE%88%E5%A4%A7%E7%9A%84%E9%97%AE%E9%A2%98%E5%B0%B1%E6%98%AF%EF%BC%8C%E8%BE%93%E5%85%A5%E6%B3%95%E7%9A%84%E5%80%99%E9%80%89%E6%A1%86%E4%B8%8D%E4%BC%9A%E8%B7%9F%E9%9A%8F%E5%85%89%E6%A0%87%EF%BC%8C%E5%85%B6%E5%AE%83%E7%9A%84%E6%9A%82%E6%97%B6%E6%B2%A1%E5%95%A5%E9%97%AE%E9%A2%98%EF%BC%8C%E5%8B%89%E5%BC%BA%E8%83%BD%E7%94%A8%E3%80%82)
>
> [Ubuntu设置中文环境](https://www.cnblogs.com/Thenext/p/17988588)
>
> [fcitx5官方文档](https://fcitx-im.org/wiki/Install_Fcitx_5/zh-cn)


## 安装中文环境
使用WSL中的GUI工具时，如果要使用中文界面，需要安装设置中文环境，还需要安装 Windows字体，不然就会导致GUI工具中的中文字符变成一个个的小方块
```bash
sudo apt update
# 安装中文语言包
sudo apt install language-pack-zh-hans
# 设置中文环境 这里要选择en_US.UTF-8和zh_CN.UTF-8, 并且zh_CN.UTF-8为默认语言
sudo dpkg-reconfigure locales
```
![设置中文环境](/devops/linux/wsl_set_language.png)
![设置默认语言](/devops/linux/wsl_set_default_language.png)
```bash
# 安装windows字体
cat <<EOF > /etc/fonts/local.conf
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <dir>/mnt/c/Windows/Fonts</dir>
</fontconfig>
EOF

# 刷新字体缓存
fc-cache -f -v 
# 重启 wsl
wsl --shutdown <你的wsl实例>
```

## 安装输入法
要在wsl环境下使用GUI开发工具的话，还需要安装输入法，不然无法输入中文（不需要中文可以跳过）
```bash
# 安装 fcitx5
sudo apt-get install fcitx5 fcitx5-configtool
# 安装拼音输入法
sudo apt-get install fcitx5-pinyin
# 配置配置拼音输入法
fcitx5-configtool
# 配置自启动
cat <<EOF >> ~/.profile 
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export DefaultIMModule=fcitx
fcitx-autostart &>/dev/null
EOF
# 应用配置
source ~/.profile
```

## 安装开发工具 RustRover
```bash
# 设置snap代理
sudo snap set system proxy.https="http://192.168.16.27:7890"
sudo snap set system proxy.http="http://192.168.16.27:7890"

# 安装rustrover
sudo snap install rustrover --classic
```