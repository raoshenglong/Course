# im2col的代码解读

阅读`caffe`中的源码:

```c++
 inline bool is_a_ge_zero_and_a_lt_b(int a, int b) {
   return static_cast<unsigned>(a) < static_cast<unsigned>(b);
 }
 
 template <typename Dtype>
 void im2col_cpu(const Dtype* data_im, const int channels,
     const int height, const int width, const int kernel_h, const int kernel_w,
     const int pad_h, const int pad_w,
     const int stride_h, const int stride_w,
     const int dilation_h, const int dilation_w,
     Dtype* data_col) {
   const int output_h = (height + 2 * pad_h -
     (dilation_h * (kernel_h - 1) + 1)) / stride_h + 1;
   const int output_w = (width + 2 * pad_w -
     (dilation_w * (kernel_w - 1) + 1)) / stride_w + 1;
   const int channel_size = height * width;
   for (int channel = channels; channel--; data_im += channel_size) {
     for (int kernel_row = 0; kernel_row < kernel_h; kernel_row++) {
       for (int kernel_col = 0; kernel_col < kernel_w; kernel_col++) {
         int input_row = -pad_h + kernel_row * dilation_h;
         for (int output_rows = output_h; output_rows; output_rows--) {
           if (!is_a_ge_zero_and_a_lt_b(input_row, height)) {
             for (int output_cols = output_w; output_cols; output_cols--) {
               *(data_col++) = 0;
             }
           } else {
             int input_col = -pad_w + kernel_col * dilation_w;
             for (int output_col = output_w; output_col; output_col--) {
               if (is_a_ge_zero_and_a_lt_b(input_col, width)) {
                 *(data_col++) = data_im[input_row * width + input_col];
               } else {
                 *(data_col++) = 0;
               }
               input_col += stride_w;
             }
           }
           input_row += stride_h;
         }
       }   
     }   
   }
 }
```

首先明确一下输出的`H`、`W`维度的大小的计算：

```C++
output_h = (height + top_pad + bottom_pad - (dilation_h * (kernel_h - 1) + 1)) / stride_h + 1;
output_w = (width + left_pad + right_pad - (dilation_w * (kernel_w - 1) + 1)) / stride_w + 1;
```

如果输入的形状为`CHW`，那么经过上述的变换之后，`ci * ho * wo * kh * kw`，其中`ho、wo`分别是上述的`output_h`和`output_w`。