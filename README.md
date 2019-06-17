# get_assets_filePath
获取main/assets目录下的文件路径
apk安装以后放在/data/app/**.apk，以apk形式存在，asset/res和apk在一起，并不会解压到/data/data/YourApp目录下去，所以你无法直接获取到assets的绝对路径，因为他根本没有;
所以我们只能通过文件读写的方式，写到缓存文件里面去，通过获取缓存文件路径 来读取，见如下代码 ，byte的size根据自己文件大小来确认，可自己调整。



public String getAssetsCacheFile(Context context,String fileName)   {
        File cacheFile = new File(context.getCacheDir(), fileName);
        try {
            InputStream inputStream = context.getAssets().open(fileName);
            try {
                FileOutputStream outputStream = new FileOutputStream(cacheFile);
                try {
                    byte[] buf = new byte[1024];
                    int len;
                    while ((len = inputStream.read(buf)) > 0) {
                        outputStream.write(buf, 0, len);
                    }
                } finally {
                    outputStream.close();
                }
            } finally {
                inputStream.close();
            }
        } catch (IOException e) {
           e.printStackTrace();
        }
        return cacheFile.getAbsolutePath();
    }

作者：面向未来_ 
来源：CSDN 
原文：https://blog.csdn.net/zengxx1989/article/details/33732333 
版权声明：本文为博主原创文章，转载请附上博文链接！
