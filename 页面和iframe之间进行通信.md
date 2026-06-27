<https://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html>

# 1、父给子传

①父页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>fu</title>
</head>
<body>
    <div>
        父页面
        <hr />
        <iframe src="./son.html" frameborder="0" id="iframe"></iframe>
    </div>
</body>

</html>
<script>
    window.onload = function () {
        fun();
    }
    function fun() {
        let iframe = document.getElementById("iframe");
        iframe.contentWindow.postMessage("你好", '*');
    }
</script>
```

②子页面

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>zi</title>
</head>

<body>
    <div>
        子页面
    </div>
</body>

</html>
<script>
    window.addEventListener('message', function (e) {
        console.log(e);
    }, false);
</script>
```

# 2、子给父传值

①子页面

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>zi</title>
</head>

<body>
    <div>
        子页面
    </div>
</body>

</html>
<script>
    window.onload = function(){
        window.parent.postMessage("你好!!!","*");
    }
</script>
```

②父页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>fu</title>
</head>
<body>
    <div>
        父页面
        <hr />
        <iframe src="./son.html" frameborder="0" id="iframe"></iframe>
    </div>
</body>

</html>
<script>
    window.addEventListener('message', receiveMessage);
    function receiveMessage(event) {
        console.log(event);
    }
</script>
```
