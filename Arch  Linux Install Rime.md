#Arch  Linux 安装 ibus-rime 

## 参考网站

[**default.custom.yaml**](https://gist.github.com/lotem/2309739)

[在方案選單中添加五筆、雙拼](https://github.com/rime/home/wiki/CustomizationGuide#%E5%9C%A8%E6%96%B9%E6%A1%88%E9%81%B8%E5%96%AE%E4%B8%AD%E6%B7%BB%E5%8A%A0%E4%BA%94%E7%AD%86%E9%9B%99%E6%8B%BC)

[**rime-wubi**](https://github.com/rime/rime-wubi)



## 操作方式

```shell
# 删除原rime（可选）
sudo pacman -Rs ibus-rime ibus-table ibus-qt
rm -r ~/.config/ibus

# 安装rime相关
sudo pacman -S ibus-rime ibus-table ibus-qt
# 将参考网站中的default.custom.yaml复制至~/.config/ibus/rime
vim default.custom.yaml 
# 配置
ibus-setup
```

一切正常的话，可以通过ctrl+~来切换输入法。



## 多说两句

本来以为需要安装[plum](https://github.com/rime/plum)来安装五笔输入法，后来发现不用，通过配置就可以了。