# gradient_2

%% concentration 0.5

%%
%'44.jpg'(128*128) is T1; '64.jpg'(256*256) is T2

A=imread('44.jpg');

figure, imshow(A);

% resize a 256*256 image to 128*128
I=imread('64.jpg');
B=imresize(I,0.5);

%% find gradient on x and y axis using CentralDifference method: 
% Gx =I(x+1)-I(x-1) Gy=I(y+1)-I(y-1)

% [Gx,Gy]=imgradientxy(A,'CentralDifference');
 [Gmag,Gdir]=imgradient(A,'CentralDifference');
% figure, imshowpair(Gmag, Gdir, 'montage');
% figure, imshowpair(Gx,Gy,'montage');
% axis off;

% [Gx2,Gy2]=imgradientxy(B,'CentralDifference');
 [Gmag2,Gdir2]=imgradient(B,'CentralDifference');
% figure, imshowpair(Gmag2, Gdir2, 'montage');
% figure, imshowpair(Gx2,Gy2,'montage');
% axis off;

%% Gdir is from -180 to 180, make it from 0 to 360; diff is the absolute value of gradient direction difference

g1=Gdir+180;
g2=Gdir2+180;

diff=abs(g1-g2);

%% pick the points that meet g1 and g2 are about opposite and magnitude of gradient is larger than 10(not random noise)

M=0;
N=0;

[a,b]=size(A);

for n=2:1:a-1
    for k=2:1:b-1
        if diff(n,k)<170 && diff(n,k)<190
            M=1;
        else
            M=0;
        end
        if Gmag(n,k)>10 && Gmag2(n,k)>10
            N=1;
        else 
            N=0;
        end
        if M*N==1
            A(n,k)=0;
        end
        end
end

%% show figure

figure, imshow(A);

%% edge detector - not that good

% BW=edge(A,'Canny');

% figure, imshow(BW)

%% compare with the reference - no improvement
% C=imread('4.jpg');
% BW2=edge(C,'Prewitt');

% figure, imshowpair(BW,BW2)
