hwhwd.github.com
================

个人学习记录

使用JMF将jpg保存为avi视频,从网络代码修改,其中注意点有三:
一.安装JMF并将jmf.jar等jmf自带的包加入工程(包括原有的jim2mov.jar)
二.生成视频文件时,使用"file:///d:/out.avi"样式的url, 否则报错:javax.media.NoDataSinkException: Cannot find a DataSink for: com.sun.media
三.使用saveMovie(MovieInfoProvider.TYPE_QUICKTIME_JPEG),否则几个常见播放器视频只能播放第一帧或者报错.


public static void main(String[] args) throws Exception {

		final File[] jpgs = new File("d:/imageFlash").listFiles();
		DefaultMovieInfoProvider dmip = new DefaultMovieInfoProvider("file:///d:/out.avi");//生成视频的url
		dmip.setFPS(2); // 设置每秒帧数
		dmip.setNumberOfFrames(jpgs.length); // 总帧数
		//视频宽和高，最好与图片宽高保持一直
		dmip.setMWidth(2048);
		dmip.setMHeight(1536);
		new Jim2Mov(new ImageProvider() {
			public byte[] getImage(int frame) {
				try {
					// 设置压缩比
					return MovieUtils.convertImageToJPEG((jpgs[frame]), 1.0f);
				} catch (IOException e) {
					e.printStackTrace();
				}
				return null;
			}
		//原代码使用了MovieInfoProvider.TYPE_AVI_MJPEG
		}, dmip, null).saveMovie(MovieInfoProvider.TYPE_QUICKTIME_JPEG);
		System.out.println("成功");
	}
