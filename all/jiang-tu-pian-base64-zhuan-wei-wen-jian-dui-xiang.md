# 将图片base64转为文件对象

最近项目中需要实现把图片的base64编码转成file文件的功能，然后再上传至服务器。起初是直接通过new File\(\)的方式进行转换，在各个主流的浏览器基本上都支持，Android也没问题，但是在ios系统埋了个坑，ios11.4以下的系统上传失败。定位bug发现是new File\(\)这个方法不兼容ios系统，只能另辟蹊径，最后找到一个方法就是：

                                        将base64转成blob ——&gt; blob转成file

这种方式测试通过，解决了new File\(\)不兼容ios系统问题。下面将base64转file文件两种方式的代码贴出来：

1.通过new File\(\)将base64转换成file文件，此方式需考虑浏览器兼容问题。

=============================================================

function base64ToFile\(base64\){

  let arr = base64.split\(','\), mime = arr\[0\].match\(/:\(.\*?\);/\)\[1\],

  bstr = atob\(arr\[1\]\), n = bstr.length, u8arr = **new** Uint8Array\(n\);

  while\(n--\){

      u8arr\[n\] = bstr.charCodeAt\(n\);

  }

  return **new** File\(\[u8arr\], 'base64ToFile', {type:mime}\);

}

=============================================================

2.先将base64转换成blob，再将blob转换成file文件，此方法不存在浏览器不兼容问题。

=============================================================

    //将base64转换为blob

    dataURLtoBlob: function\(dataurl\) { 

        var arr = dataurl.split\(','\),

            mime = arr\[0\].match\(/:\(.\*?\);/\)\[1\],

            bstr = atob\(arr\[1\]\),

            n = bstr.length,

            u8arr = new Uint8Array\(n\);

        while \(n--\) {

            u8arr\[n\] = bstr.charCodeAt\(n\);

        }

        return new Blob\(\[u8arr\], { type: mime }\);

    },

    //将blob转换为file

    blobToFile: function\(theBlob, fileName\){

       theBlob.lastModifiedDate = new Date\(\);

       theBlob.name = fileName;

       return theBlob;

    },

    //调用

    var blob = dataURLtoBlob\(base64Data\);

    var file = blobToFile\(blob, imgName\);

=============================================================

