## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/eunjeeee/matlab.github.io/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```matlab
clc;
clear;

% Problem 1
%A = imread('./data/banana_slug.cr2');
A = imread('./data/banana_slug.tiff');
class(A)
size(A)
trans_A = double(A);
%imshow(A)

%min(min(trans_A))
%max(max(trans_A))



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

%new_trans_A = uint8(new_trans_A*256);
% ‘grbg’, ‘rggb’, ‘bggr’, ‘gbrg’
%figure(2)
%bA = demosaic(new_trans_A, 'grbg');
%bB = demosaic(new_trans_A, 'rggb');
%bC = demosaic(new_trans_A, 'bggr');
%bD = demosaic(new_trans_A, 'gbrg');




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


%% Problem 5

di_r = interp2(im_whiteworld(:,:,1));
di_g = interp2(im_whiteworld(:,:,2));
di_b = interp2(im_whiteworld(:,:,3));
im_di = cat(3, di_r, di_g, di_b);
figure; imshow(im_di);


%% Problem 6

if im_di < 0.0031308
    c_non = im_di * 12.92;
else 
    c_non = (1+0.055)*(im_di.^(1/2.4)) - 0.055;
end
figure; imshow(c_non);



%% Problem 7

imwrite(c_non, 'A.png');
imwrite(c_non, 'A_50.jpeg', 'quality', 50);
imwrite(c_non, 'A_30.jpeg', 'quality', 30);
imwrite(c_non, 'A_20.jpeg', 'quality', 20);
imwrite(c_non, 'A_15.jpeg', 'quality', 15);
imwrite(c_non, 'A_10.jpeg', 'quality', 10);
imwrite(c_non, 'A_5.jpeg', 'quality', 5);


```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/eunjeeee/matlab.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
