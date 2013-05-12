# man page for gpg-man
## NAME 
**gpg-man** manages the **gpg** key database. It also supports encrypting, decrypting, signing and verifying signatures with **gpg**
## SYNOPSIS
**gpg-man** [objectPath] [ objectId1 ... objectIdN ] -action [ actionArg1 .. actionArgM]

## DESCRIPTION 
### db
Use this object path to manipulate the entire database in one go, mostly for backing up, restoring, or deleting the entire database of keys.

### key
A key is always identified by an email address. It may have a **private** and a **public** part.

### key.private
You can use a private key to create a signature file for a file that you want to send. 

### key.public
You can use a public key to encrypt messages that you send. 

### keys
At the level of the keys collection, you can import public and private keys. If keys were managed outside **gpg-man**, you can fix their trust level, so that they become usable again.

### keys.private
At the level of the collection of private keys, you can decrypt a message that you receive.

### keys.public
At the level of the collection of public keys, you can verify the signature of a file.

### stash
before making changes to the database, you may want to ensure that you can undo your changes. Stash your database. At any time you can always unstash back its stashed version.

## OPTIONS

### --help
shows the general help paragraph.

### [objectpath] --help
shows general help for the object path.

### --version
shows the program's version.

### --license
shows the program's license.

### db -stash
This command will create a local copy of the gpg user-level configuration database. Use it if you are not sure what effect your changes will have. You can put back the version stashed with: '**gpg-man** db -unstash'.

### db -restore [arg]
This command restores the gpg user-level configuration database from the zip file supplied as an argument. Example:  
    **gpg-man** db -restore mybackup.zip  
will restore mybackup.zip.

### db -backup [arg]
This command creates a zip backup of the ssh user-level configuration database and saves it to the filepath supplied in [arg].
Examples:  
    **gpg-man** db -backup mybackup  
will create mybackup.zip  
    **gpg-man** db -backup mybackup.zip  
will also create mybackup.zip  
The backup zip contains all user-level configuration database files used by **gpg**.

### db -delete
This command deletes the entire gpg user-level configuration database.

### db -unstash
This command puts back the copy of the gpg user-level configuration database made earlier with '**gpg-man** db -stash'. Use it if you want to undo the changes made since the last time you stashed the database.

### key [obj] -create [arg]
This command creates a key for given email address and a name. Example:  
        **gpg-man** key john@doe.com -create "John Doe"  
If the name contains spaces, you should enquote the name or else escape the quote. Acceptable alternatives, are:  
        **gpg-man** key john@doe.com -create 'John Doe'  
        **gpg-man** key john@doe.com -create John\ Doe  
Key creating is notoriously lengthy with **gpg**. Also, don't be surprised that the program keeps accepting input while it is creating the key. This was done on purpose in **gpg**, in order to increase the entropy (randomness) of the key.


### key [obj] -delete
This command deletes a key for a given email address. Example:  
        **gpg-man** key john@doe.com -delete

### key.private [obj] -show
This command dumps the private key for an email address to stdout. Example:  
        **gpg-man** key.private john@doe.com -show > john.doe.key.pri  
The redirection will save the key into the file 'john.doe.key.pri'. Without redirection, the command will just show the content of the key on the terminal screen. Example:  
        **gpg-man** key.private john@doe.com -show


### key.private [obj] -exists
This command verify if a private key exists for a particular email address. Example:  
        **gpg-man** key.private john@doe.com -exists
The command outputs 'yes' if the private key is present and 'no' if not.

### key.private [obj] -delete
This command deletes only the private key for a given email address. Example:  
        **gpg-man** ke.private john@doe.com -delete

### key.private [obj] -sign [arg]
This command uses the private key for an email address to sign a file. Example:  
        **gpg-man** key.private john@doe.com -sign myfile.doc > myfile.sig  
The redirection will save the signature into the file 'myfile.sig'. Without redirection, the command will just show the signature on the terminal screen. Example:  
        **gpg-man** key.private john@doe.com -sign myfile.doc  
It is also possible to supply the file on stdin. Example:  
        cat myfile.doc | **gpg-man** key.private john@doe.com -sign - > myfile.sig  
In that case, the file argument should be a dash ("-").

### key.public [obj] -exists
This command verify if a public key exists for a particular email address. Example:  
        **gpg-man** key.public john@doe.com -exists
The command outputs 'yes' if the public key is present and 'no' if not.
 
### key.public [obj] -encrypt [arg]
This command uses the public key for an email address to encrypt a file. Example:  
        **gpg-man** key.public john@doe.com -encrypt myfile.doc > myfile.doc.encrypted  
The redirection will save the key into the file 'myfile.doc.encrypted'. Without redirection, the command will just show the encrypted file on the terminal screen. Example:  
        **gpg-man** key.public john@doe.com -encrypt myfile.doc  
It is also possible to supply the file on stdin. Example:  
        cat myfile.doc | **gpg-man** key.private john@doe.com -encrypt - > myfile.doc.encrypted  
In that case, the file argument should be a dash ("-").

### key.public [obj] -show
This command dumps the public key for an email address to stdout. Example:  
        **gpg-man** key.public john@doe.com -show > john.doe.key.pri  
The redirection will save the key into the file 'john.doe.key.pri'. Without redirection, the command will just show the content of the key on the terminal screen. Example:  
        **gpg-man** key.public john@doe.com -show

### key.public [obj] -fix-trust
This command restores the trust in the key in the database identified by an email address, to the maximum level (6). At lower levels, **gpg** may balk at using the key. Example:  
        **gpg-man** key john@doe.com -fix-trust  
**gpg-man** will normally always store keys at maximum trust level. It is possible, however, to manage the **gpg** database with other tools or directly with **gpg** and use different levels of ownertrust. 

### keys -import-private-key [arg]
This command imports a private key into the database. Example:  
        **gpg-man** db -import-private-key myfile.pri  
In order to import from stdin, use '-' as file name. Example:  
        cat mykey.pri | **gpg-man** db -import-private-key -  

### keys -import-public-key [arg]
This command imports a public key into the database. Example:  
        **gpg-man** db -import-public-key myfile.pub
In order to import from stdin, use '-' as file name. Example:  
        cat mykey.pub | **gpg-man** db -import-public-key -  

### keys -show
This command shows the list of keys stored in the database. Example:  
  
$ **gpg-man** keys -show  
email                          id         trust   public  private name  
john1@doe.com                  5E15E3DA   6       yes     yes     John 1 Doe  
john2@doe.com                  7E15E3DA   6       yes     yes     John 2 Doe  
john3@doe.com                  9E15E3DA   6       yes     yes     John 3 Doe  
  
**email**: The email address for the key.  
**id**: The id for the key. **gpg** often refers to the id when referring to a key.  
**trust**: The trust level for a key. **gpg-man** only deals with keys at trust level 6. Keys at lower trust levels may not always be usable.  
**public**: Yes, if the public key is present. It should always be present.  
**private**: Yes, if the private key is present. A key with just a public part, is very normal, if it was supplied by someone else to you.  
**name**: The name of the person associated to the key.

### keys -fix-trust
This command restores the trust in every key in the database to the maximum level (6). At lower levels, **gpg** may balk at using the key. Example:  
        **gpg-man** db -fix-trust  
**gpg-man** will normally always store keys at maximum trust level. It is possible, however, to manage the **gpg** database with other tools or directly with **gpg** and use different levels of ownertrust. 


### keys.private -decrypt [arg]
This command scans through the collection of private keys to locate a private key than can decrypt the given message. Example:  
  
**gpg-man** keys.private john@doe.com -decrypt myfile.doc.encrypted > myfile.doc  
  
The redirection will save the cleartext content into the file 'myfile.doc'. Without redirection, the command will just show the encrypted file on the terminal screen. Example:  
  
**gpg-man** keys.private john@doe.com -decrypt myfile.doc.encrypted  
  
It is also possible to supply the file on stdin. Example:  
  
cat myfile.doc | **gpg-man** keys.private john@doe.com -decrypt -  > myfile.doc  
  
In that case, the file argument should be a dash ("-").

### keys.public -verify-sig [args]
This command scans through the collection of public keys to locate a public key than can verify the signature of the given message. Example:  
        **gpg-man** keys.public -verify-sig myfile.sig myfile.doc  
The first argument must be the signature file (myfile.sig). The second argument must be the file which to which the signature applies (myfile.doc).

### stash -exists
This command checks if the stash exists. It outputs "yes", if it exists, and "no", if it doesn't.

### stash -delete
This command deletes the stash.

## ENVIRONMENT 
### GPG_FOLDER
By default, **gpg-man** will look for the gpg database in ~/.gnupg. You can override this with the GPG_FOLDER environment variable. Example:  
    GPG_FOLDER=/var/test/mygpg **gpg-man** ...
Setting the variable just before invoking **gpg-man** will point the GPG_FOLDER elsewhere.
### GPG_STASHED_FOLDER
By default, **gpg-man** will stash the database in ~/.gnupg.stashed. You can override this with the GPG_STASHED_FOLDER environment variable. Example:  
    GPG_STASHED_FOLDER=/var/test/mygpgstash **gpg-man** ...

## EXIT-STATUS 
### 0
A zero exit code means that everything went ok.
### 1
A one exit code is usually an error generated by **gpg-man** itself.
### other
Another exit code is always an error generated by one of the underlying programs invoked by **gpg-man**.

## FILES 
By default, **gpg-man** looks for the gpg security database in **~/.gnupg**.
### pubring.gpg
This file contains the public keys.
### secring.gpg
This file contains the private keys.
### trustdb.gpg
This file contains the trust levels for the keys.

## AUTHOR
Erik Poupaert <erik@sankuru.biz>

## REPORTING-BUGS
Report bugs to: erik@sankuru.biz

# COPYRIGHT
Licensed under: GPL
