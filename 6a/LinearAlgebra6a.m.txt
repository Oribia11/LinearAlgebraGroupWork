% Specify the file path of the image
imagePath = "C:\Users\20232862\OneDrive - TU Eindhoven\Pictures\Saved Pictures\Coach Mikki and Friends.jpg";

% Read the color image
img_color = imread(imagePath);

% Convert the color image to grayscale
img_gray = rgb2gray(img_color);

% Convert the grayscale image to double precision for numerical stability
A = im2double(img_gray);

% Calculate the SVD
[U, S, V] = svd(A);

% Choose three values of k for TSVD
k_values = [10, 25, 50];

% Display the original image
figure;
subplot(2, 2, 1);
imshow(A);
title('Original Grayscale Image');

% Perform TSVD for selected values of k and display the reconstructed images
for i = 1:length(k_values)
    k = k_values(i);
    
    % Truncate singular values and reconstruct
    Ak = U(:, 1:k) * S(1:k, 1:k) * V(:, 1:k)';
    
    % Display the reconstructed image
    subplot(2, 2, i + 1);
    imshow(Ak);
    title(['TSVD Reconstruction (k = ' num2str(k) ')']);
end

% Compute the savings in storage (assuming each number takes 1 byte)
original_size = numel(A);
savings = zeros(size(k_values));

for i = 1:length(k_values)
    k = k_values(i);
    
    % Size of matrices in TSVD
    Uk_size = numel(U(:, 1:k));
    Sk_size = numel(S(1:k, 1:k));
    Vk_size = numel(V(:, 1:k));
    
    % Total size in TSVD
    total_size_tsvd = Uk_size + Sk_size + Vk_size;
    
    % Savings
    savings(i) = 1 - total_size_tsvd / original_size;
end

% Display the savings
disp('Savings in Storage:');
disp(['k = ' num2str(k_values(1)) ': ' num2str(savings(1) * 100) '%']);
disp(['k = ' num2str(k_values(2)) ': ' num2str(savings(2) * 100) '%']);
disp(['k = ' num2str(k_values(3)) ': ' num2str(savings(3) * 100) '%']);
