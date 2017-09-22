---
layout: post
title: Product Cipher - Bridging Classic and Modern
tags: [Computer Security]
---

The Product cipher implementation was one of the mini projects that I had to complete for the Computer Security.
We will be encrypting a simple text file with **Secret message** :D.

![Secret Message]({{ site.baseurl }}/images/product-cipher/sample-message.png "Secret Message")

Product cipher is considered as a bridge between the modern and classical cipher. Product cipher incorporates several aspects from the classical
cipher such as the substitution and permutations. In the implementation, we will be implementing a system that will incorporate both substitution and permutation.

<img src="{{ site.baseurl }}/images/product-cipher/classical-cipher-disturbing.jpg" width="50%" alt="Vader classical cipher disturbing" title="Vader classical cipher disturbing">

### Getting off

Before the implementation of the application, we will have to create the interfaces and the controllers for the application which will contain
no logic relating to the cipher implementation.

I have implemented these requirements in controller, main and view packages and they will be really helpful as a starting point if you are not planning do everything from scratch.

- lk.bhanuka.cipher.controller  - All the controller logic of the application
- lk.bhanuka.cipher.main        - Entry points to the application
- lk.bhanuka.cipher.view        - The View components of the application

In addition we will be coding in a core package which will contain all the logic relating to the encryption and decryption.

### Transfer objects

We will be using the concept of transfer objects to pass information between the encryption and decryption processes. Hence we will require transfer object classes for encryption and decryption requests. For the moment we will be including only the file specifications in these transfer objects.

``` java
package lk.bhanuka.cipher.core;

import java.io.File;

/**
 *
 * @author bhanuka
 */
public class EncryptionRequest {

    public File inputFile;
}
```

### Getting on with core

The encryptor will have the responsibility of encrypting the file provided through the encryption request and the encryption process will include a permuting and substituting phase.

``` java

  public void encrypt(EncryptionRequest request) throws IOException{

        List<String> lines = this.fileReader.readFileLines(request.inputFile);

        List<String> encrypted = new ArrayList();

        for(String line : lines){

            System.out.println("Encryting line : "+ line);

            encrypted.add(this.encrypt(line));
        }

        this.fileWriter.writeFileLines("encrypted-"+request.inputFile.getName(), encrypted);

    }

    private String encrypt(String text){
        return this.permute(this.substitute(text));
    }
  ```
##### Substitution

The substitution process will have seperate substitution mechanisam for characters in odd and even indices. In the example we have used simple increment of the ASCII value with `key1` and `key2`.

``` java

private String substitute(String text){
    int key1 = 2;
    int key2 = 3;

    StringBuilder builder = new StringBuilder();

    for(int i =0; i < text.length(); i++){

        char letter = text.charAt(i);

        if(i%2 == 0){
            builder.append((char) (letter+key1));
        }
        else{
            builder.append((char) (letter+key2));
        }
    }

    return builder.toString();
}
```

##### Permutation

The permutation process, the substituted string will be converted to a matrix with `key3` width and the matrix will be returned in column-wise manner.

``` java
private String permute(String text){

        int key = key3;

        int rows = text.length()/key;

        if(!(text.length() % key == 0)){
            rows++;
        }

        int columns = key;       

        char matrix[][]=new char[rows][columns];

        int a=0;
        for(int i=0;i<rows;i++){
            for(int j=0;j<columns;j++){
                if(a<text.length()){
                    matrix[i][j]=text.charAt(a);
                }
                a++;
            }
        }

        StringBuilder builder = new StringBuilder();
        for(int j=0;j<columns;j++){
            for(int i=0;i<rows;i++){
                    builder.append(matrix[i][j]);
            }
        }        

        return builder.toString();
    }
```

The complete reverse process of the above process has to be implemented in the case of the Decryptor, which is relatively easier compared to the Encryptor which we have built.

### Testing the Application

- Complete the application following the instructions, you might require some of the implementations of the FileReaders and FileWriters, which are avaialable in the repository.

- To test the application we will be using a sample message file (.txt file) containing the following secret message.

`This is a sample message containing confidential information to some user. Here the message contained in the file will be encrypted using product cipher and again decrypted to obtain the same message back.`

- The encrypted file will be avaialable in the base directory of the Application with name `encrypted-file-name.txt`.

- Decrypt the encrypt file to test the Encryptor and you should get the same message back. ( Double check the Decryptor process with the Encryptor if you are not)

![Encrypted Secret Message]({{ site.baseurl }}/images/product-cipher/encrypted-sample-message.png "Encrypted Secret Message")


![Decrypted Encrypted Secret Message]({{ site.baseurl }}/images/product-cipher/decrypted-encrypted-sample-message.png "Encrypted Secret Message")

### Improving the Application

- Move the `key1`, `key2` and `key3` to a configuration file or prompt the user to enter the settings.
- Improve the Application to encrypt any type of file.


### Source

[https://github.com/mahanama94/ProductCipher](https://github.com/mahanama94/ProductCipher "Source Code")


<img src="{{ site.baseurl }}/images/product-cipher/encrypt-all-the-things.jpg" width="50%">

Cheers !!!

**mahanama94**
