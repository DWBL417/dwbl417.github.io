---
layout:     post
title:      为什么我不推荐你使用免费VPN
category:   dump
description: 02-10 | Ctrl+A
---

实在懒得写了，简单说一说吧。

VPN加密一点问题没有，但它的流量特征太明显了，这就像在夜店里大家都穿的很少，然后你身着防弹衣入场了。安全问题解决了，但所有人都知道你不是来跳舞的。

这是所有VPN的通病，在别人眼里虽然不知道你在浏览什么，但都知道你在干什么。

然而，那些你手机上免费的VPN软件比这更惨一些，不仅做不到隐身，安全也难以保障。因为你不知道连接的那一端是否可靠，可能是蜜罐，或者是偷窥狂人，你的传输内容就会被一览无余。你可能会说我用了很久都没有问题，我只能说这涉及到一个成本问题。服务器是要烧钱的，人越多越烧钱。你想想，它的利益从哪来，你仔细想想。

----


## <font color="#ffffff">哈！被你发现了～</font><br />
# <font color="#ffffff">在「城堡」页面中藏有彩蛋，依次按键“上下左右BA”发动秘技！（30日内有效）</font><br />
# <font color="#ffffff">  </font><br />

----



<script language="javascript">
var allowedKeys = {
  37: 'left',
  38: 'up',
  39: 'right',
  40: 'down',
  65: 'a',
  66: 'b'
};
var konamiCode = ['up', 'down', 'left',  'right', 'b', 'a' ];

var konamiCodePosition = 0;

// add keydown event listener
document.addEventListener('keydown', function(e) {
  // get the value of the key code from the key map
  var key = allowedKeys[e.keyCode];
  // get the value of the required key from the konami code
  var requiredKey = konamiCode[konamiCodePosition];

  // compare the key with the required key
  if (key == requiredKey) {

    // move to the next key in the konami code sequence
    konamiCodePosition++;

    // if the last key is reached, activate cheats
    if (konamiCodePosition == konamiCode.length) {
      activateCheats();
      konamiCodePosition = 0;
    }
  } else {
    konamiCodePosition = 0;
  }
});

function activateCheats() {
  alert("要在「城堡」里才有效哦！");
  window.location.href="https://publius.club/castle";
}
