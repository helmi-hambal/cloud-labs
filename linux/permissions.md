# Linux: File Permissions & Ownership (Lab 2)

## Permission Basics
- rwx → read, write, execute
- u (user/owner), g (group), o (others), a (all)

## chmod Examples
chmod o-r file1.txt      # remove read from others
chmod u+x file1.txt      # add execute for owner
chmod 755 file1.txt      # rwx for owner, r-x for group & others

## chown & chgrp
sudo chown demo1 file1.txt
sudo chgrp sudo file1.txt

## Special Bits
chmod +t shared_dir      # sticky bit
chmod g+s project_dir    # SGID

