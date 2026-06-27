# 1、文字显示省略号

1）文字两行显示，超出部分显示省略号

```css
容器{
    width:200px;
    /* 高度要是两行文字的高 */
    height: 40px;
    border:1px solid red;
    overflow: hidden;
    -webkit-line-clamp:2;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
}
```

2）单行显示，超出显示省略号

```html
容器{
    /* 要给容器的宽 */
    width:200px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
```

# 2、修改滚动条

```css
/*滚动条*/
::-webkit-scrollbar {
    width: 7px;
    height: 8px;
    background-color: rgba(0, 0, 0, 0);
}
/*滚动条的轨道*/
::-webkit-scrollbar-track {
    background-color: rgba(255, 255, 255, 0);
}
/*滚动条的滑块按钮*/
::-webkit-scrollbar-thumb {
    border-radius: 10px;
    width: 8px;
    height: 8px;
    background-color: #cfd7e0;
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.1);
}
/*滚动条的上下两端的按钮*/
::-webkit-scrollbar-button {
    height: 0px;
    width: 0px;
    background-color: #cfd7e0;
}
```
