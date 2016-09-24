# Rem、1px、android

目前公司的项目采用了rem布局，为了实现等比例还原效果图和“real 1px”。我们的方案在IOS设备中（7.0+）效果均表现良好，但在Android Webkit下有严重bug。

经过排查后，发现是由于Android Webkit中的initial-scale存在bug，只能设置数值1，bug说明详见http://www.quirksmode.org/mobile/metaviewport/





