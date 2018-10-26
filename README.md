# 音訊檔調速/AM調變/FM調變

熟習Matlab/Octave，與錄放音程式，並實際產生與處理數位訊號，觀察其頻譜

### 要求

1.錄音：利用wavesurfer或cooledit錄數種不同種類的聲音，例如說話聲，歌唱聲，音樂，背景雜訊，並畫出其頻譜。

2.MATLAB操作：對讀進來之音檔資料，進行數學運算（最大值、最小值、平均值、快慢轉），然後試聽結果。

3.程式改寫：參考範例程式FM1、FM2改寫出以AM及FM調變後的任意音檔之結果。

### 檔案解釋

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/all.PNG)

* derjk.wav：欲讀入之人聲檔。
* hu.mp3：欲讀入之音樂檔。
* s105820018.m：讀檔並找出最大值、最小值及平均值，執行後會輸出三個不同速度、振幅的音檔“huX.wav”，兩張圖分別顯示人聲檔及音樂檔的波形、頻譜分析、以及頻域。
* t105820018_am.m：讀入人聲檔並做載波頻率為30Hz的AM調變，輸出modulation signal、carrier signal、AM signal及頻域圖形。
* t105820018_fm.m：讀入人聲檔並做載波頻率為30Hz的FM調變，輸出modulation signal、carrier signal、FM signal及頻域圖形。

### 程式碼清單

s105820018.m

```
[x fs1]=audioread('hu.mp3');
figure;
set(figure(1),'NumberTitle','off','Name','BGM') 
title('BGM')
subplot(2,2,1);
plot(x(1:100000,:))
subplot(2,2,2);
x2=fftshift(fft(x));
plot(abs(x2))
subplot(2,2,3);
spectrogram(x(:,1),'yaxis')
subplot(2,2,4);
specgram(x(:,1))

[y fs2]=audioread('derjk.wav');
figure;
set(figure(2),'NumberTitle','off','Name','Recording') 
subplot(2,2,1);
plot(y(1:100000,:))
title('derjk')
subplot(2,2,2);
y2=fftshift(fft(y));
plot(abs(y2))
subplot(2,2,3);
spectrogram(y(:,1),'yaxis')
subplot(2,2,4);
specgram(y(:,1))

MIN=min(x);   
MAX=max(x);  
MEAN=mean(x);  
X=x*10;    
audiowrite('hu1.wav',X,fs1);  
audiowrite('hu2.wav',x,fs1*0.5);
audiowrite('hu3.wav',x,fs1*1.5);
```

t105820018_am.m

```
[ym fm]=audioread('derjk.wav');
ym=ym*70;
ym=ym';
ym(1,:)=[];
ym(144002:length(ym))=[];
t = 0:1/fm:3;
subplot(3,1,1);
plot(t,ym)
title('modulation signal');

Ac=1;
fc=30;
yc=Ac*cos(2*pi*fc*t);
subplot(3,1,2)
plot(t,yc)
title('carrier signal');

y=Ac.*(1+1*ym).*cos(2*pi*fc*t);
subplot(3,1,3)
plot(t,y)
title('AM signal')

figure;
plot(linspace(-fm/2,fm/2,length(y)),abs(fftshift(fft(y))));
```

t105820018_fm.m

```
[ym fm]=audioread('derjk.wav');
ym=ym';
ym(1,:)=[];
ym(6002:length(ym))=[];
fm=fm/24;
t = 0:1/fm:3;
subplot(3,1,1);
plot(t,ym)
title('modulation signal');

Ac=1;
fc=30;
yc=Ac*cos(2*pi*fc*t);
subplot(3,1,2)
plot(t,yc)
title('carrier signal');

y=Ac*cos(2*pi*fc*t + cumsum(2*pi*1*ym));
subplot(3,1,3)
plot(t,y)
title('FM signal')

figure;
plot(linspace(-fm/2,fm/2,length(y)),abs(fftshift(fft(y))));
```

### 輸出結果

s105820018.m

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/hu1.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/hu2.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/hu3.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/hu4.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/derjk1.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/derjk2.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/derjk3.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/derjk4.PNG)

t105820018_am.m

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/AM1.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/AM2.PNG)

t105820018_fm.m

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/FM1.PNG)

![image](https://github.com/DigitalSignalProcessingNTUT2018/basic-signal-and-specgram-105820018He/blob/master/ImageForREADME/FM2.PNG)