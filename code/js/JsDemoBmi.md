# 利用javascript进行计算 BMI
- date: 2021-04-17T
- lastmod: 2021-04-17

# BMI

[BMI 指数（Body Mass Index），是国际上常用的衡量人体胖瘦程度以及是否健康的一个标准。](https://baike.baidu.com/item/%E6%A0%87%E5%87%86%E4%BD%93%E9%87%8D/1694152)

## 源代码

基于MIT协议开放的源代码位于gitee仓库[ Kearney / JsDemo](https://gitee.com/anidea/js-demo/tree/master/bmi)，本想着是打包成微信小程序的，但是一个邮箱只能整一个小程序嘛，之前的小程序被冻结了，剩下的一个邮箱又注册了微信公众号...

## 标准

|   BMI	|   分类	|   建议	|
|:---:	|:---:	    |:---:	    |
| <18.5 |   偏瘦	|   多吃点	|
| <24  	|   正常	|   继续保持	|
| <28  	|   偏胖	|   加强锻炼	|
| ≥28 	|   肥胖	|   慢慢来	|

ps：尊重别人的身体，就像你自己的身体一样，避免采用带有歧视性或者侮辱性的词汇，不然我们将保留关（bao）切（da）你的权力

# 计算

<div>
<label>身高（cm）</label>
    <input id='height' value='170' onkeyup="calbmi()" required>
    <br>
    <label>体重（kg）</label>
    <input id='weight' value='65' onkeyup="calbmi()" required>
    <br>
    <label>BMI</label>
    <input id='res' disabled>
</div>


<script type='text/javascript'>
    // 计算 BMI
    function calbmi() {
        var height = document.getElementById("height").value / 100;
        var weight = document.getElementById("weight").value;
        var bmi = weight / Math.pow(height, 2);
        console.log(bmi);
        document.getElementById("res").value = bmi.toFixed(1);
    }
    calbmi()
</script>
