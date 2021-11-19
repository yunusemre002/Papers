
`ls -lh`	the apparent space of the directory and true space of the files and in details the names, creation date of each.  
`ls -s` 	to list file size,  
`ls -sh` 	for human readable sizes.  
`du -h` 	for human readable sizes for directories  
  
* find needs a starting point, and the . (dot) points to the current directory.  
`find . -name "foo*"`

`chown yunus file` --> Changes the ownership of the file from its current owner to user bob.

* To install a .deb file by opening a terminal and typing:
  `sudo dpkg -i package_file.deb`
  
* `chmod 777 file_name` çalıştırma izni verir. Eğer bir directory ise ve alt file lar için de izin isteniyorsa `chnod -R 777 fine_name` denilmeli.

### nano [file](https://www.nano-editor.org/dist/latest/cheatsheet.html)
* **Ctrl+K**   	Cut current line into cutbuffer
* **Alt+6**	Copy current line into cutbuffer
* **Ctrl+U**	Paste contents of cutbuffer
* **Alt+T**	Cut until end of buffer
* **Ctrl+]**	Complete current word
* **Alt+3**	Comment/uncomment line/region
* **Alt+U**	Undo last action
* **Alt+E**	Redo last undone action

* **Alt+Del**	Delete current line
