# Homework Assignment 1

<p align='center'>
  <img src='https://github.com/eunjeeee/matlab/blob/gh-pages/image/banana_slug.tiff.PNG' width="600px">
<p align='center'>
original image (banana_slug.tiff)
  
### INITIALS
```matlab
%% Problem 1

%A = imread('./data/banana_slug.cr2');
A = imread('./data/banana_slug.tiff');
class(A)
size(A)
trans_A = double(A);
```

### LINEARIZATION
```matlab
%% Problem 2

% 0 - 16383 => 2047 - 15000
% x < 2047 = 0
% x > 15000 = 16383

min_A = 2047*ones(size(A));
max_A = 15000*ones(size(A));

new_A = max(trans_A, min_A);
new_A = min(new_A, max_A);

new_trans_A = (new_A - 2047)/(15000 - 2047);
figure(1)
imshow(new_trans_A)
```

### BAYER PATTERN
```matlab
%% Problem 3

im1=new_trans_A(1:2:end, 1:2:end); %R
im2=new_trans_A(1:2:end, 2:2:end); %G1
im3=new_trans_A(2:2:end, 1:2:end); %G2
im4=new_trans_A(2:2:end, 2:2:end); %B

% ‘grbg’, ‘rggb’, ‘bggr’, ‘gbrg’

im_grbg = cat(3, im2, im1, im3); 
im_rggb = cat(3, im1, im2, im4); 
im_bggr = cat(3, im4, im2, im1); 
im_gbrg = cat(3, im3, im1, im2); 

figure(2)
subplot(2,2,1)
imshow(im_grbg)
subplot(2,2,2)
imshow(im_rggb)
subplot(2,2,3)
imshow(im_bggr)
subplot(2,2,4)
imshow(im_gbrg)

figure; imshow(min(1, im_rggb * 5));
```

### WHITE BALANCING
```matlab
%% Problem 4

grey_r = mean(mean(im_rggb(:,:,1)));
grey_g = mean(mean(im_rggb(:,:,2)));
grey_b = mean(mean(im_rggb(:,:,3)));
im_greyworld = cat(3,im_rggb(:,:,1)*(grey_g/grey_r), im_rggb(:,:,2), im_rggb(:,:,3)*(grey_g/grey_b));
white_r = max(max(im_rggb(:,:,1)));
white_g = max(max(im_rggb(:,:,2)));
white_b = max(max(im_rggb(:,:,3)));
im_whiteworld = cat(3,im_rggb(:,:,1)*(white_g/white_r), im_rggb(:,:,2), im_rggb(:,:,3)*(white_g/white_b));

figure; imshow(im_greyworld);
figure; imshow(im_whiteworld);
```

### DEMOSAICING
```matlab
%% Problem 5

di_r = interp2(im_whiteworld(:,:,1));
di_g = interp2(im_whiteworld(:,:,2));
di_b = interp2(im_whiteworld(:,:,3));
im_di = cat(3, di_r, di_g, di_b);
figure; imshow(im_di);
```

### GAMMA CORRECTION
```matlab
%% Problem 6

if im_di < 0.0031308
    c_non = im_di * 12.92;
else 
    c_non = (1+0.055)*(im_di.^(1/2.4)) - 0.055;
end
figure; imshow(c_non);
```

### COMPRESSION
```matlab
%% Problem 7

imwrite(c_non, 'A.png');
imwrite(c_non, 'A_95.jpeg', 'quality', 95);
imwrite(c_non, 'A_50.jpeg', 'quality', 50);
imwrite(c_non, 'A_30.jpeg', 'quality', 30);
imwrite(c_non, 'A_15.jpeg', 'quality', 15);
imwrite(c_non, 'A_10.jpeg', 'quality', 10);
imwrite(c_non, 'A_5.jpeg', 'quality', 5);
```

