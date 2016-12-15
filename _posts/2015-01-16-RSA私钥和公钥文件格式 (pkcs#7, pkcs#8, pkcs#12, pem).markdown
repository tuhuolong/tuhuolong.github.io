---
layout: post
title: "RSA私钥和公钥文件格式 (pkcs#7, pkcs#8, pkcs#12, pem)"
date: 2015-01-16 16:37:00 
comments: true
categories: []
tags: []
description: "RSA私钥和公钥文件格式 (pkcs#7, pkcs#8, pkcs#12, pem)"
keywords: 
---


 
  
   
    
     
      Format
     
     
      Name
     
     
      Description
     
    
    
     
      PKCS #7
     
     
      Cryptographic Message Syntax Standard
     
     
      A PKCS #7 file can be used to store certificates, which is a SignedData structure without data (just the certificates). The file name extension is usually
      
       .p7b
      
      ,
      
       .p7c
      
     
    
    
     
      PKCS #8
     
     
      Private-Key Information Syntax Standard.
     
     
      Used to carry private certificate keypairs (encrypted or unencrypted).
     
    
    
     
      PKCS #12
     
     
      Personal Information Exchange Syntax Standard.
     
     
      Defines a file format commonly used to store private keys with accompanying public key certificates, protected with a password-based symmetric key. It is the successor to PFX from Microsoft.
     
    
    
     
      DER
     
     
      Distinguished Encoding Rules
     
     
      A binary format for keys or certificates. It is a message transfer syntax specified by the ITU in X.690.
     
    
    
     
      PEM
     
     
      Privacy Enhanced Mail
     
     
      Base64 encoded DER certificates or keys, with additional header and footer lines.
      
       
        The PEM private key format uses the header and footer lines:
        
         
          -----BEGIN RSA PRIVATE KEY-----
         
         
          
           -----END RSA PRIVATE KEY-----
          
          
           
            The PEM public key format uses the header and footer lines:
            
             
              -----BEGIN PUBLIC KEY-----
             
             
              
               -----END PUBLIC KEY-----
              
              
               
                The PEM certificate uses the header and footer lines:
                
                 
                  -----BEGIN CERTIFICATE-----
                 
                 
                  
                   -----END CERTIFICATE-----
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
  
  
 
 
 
 
  RSA Public Key file (PKCS#1)
 
 
 
 
  The RSA Public key PEM file is specific for RSA keys.
 
 
  It starts and ends with the tags:
 
 -----BEGIN RSA PUBLIC KEY-----
BASE64 ENCODED DATA
-----END RSA PUBLIC KEY-----

 
  Within the base64 encoded data the following DER structure is present:
 
 RSAPublicKey ::= SEQUENCE {
    modulus           INTEGER,  -- n
    publicExponent    INTEGER   -- e
}

 
 
 
  Public Key file (PKCS#8)
 
 
  Because RSA is not used exclusively inside X509 and SSL/TLS, a more generic key format is available in the form of PKCS#8, that identifies the type of public key and contains the relevant data.
 
 
  It starts and ends with the tags:
 
 -----BEGIN PUBLIC KEY-----
BASE64 ENCODED DATA
-----END PUBLIC KEY-----

 
  Within the base64 encoded data the following DER structure is present:
 
 PublicKeyInfo ::= SEQUENCE {
  algorithm       AlgorithmIdentifier,
  PublicKey       BIT STRING
}

AlgorithmIdentifier ::= SEQUENCE {
  algorithm       OBJECT IDENTIFIER,
  parameters      ANY DEFINED BY algorithm OPTIONAL
}

 
  So for an RSA public key, the OID is 1.2.840.113549.1.1.1 and there is a RSAPublicKey as the PublicKey key data bitstring.
 
 
 
 
  RSA Private Key file (PKCS#1)
 
 
  The RSA private key PEM file is specific for RSA keys.
 
 
  It starts and ends with the tags:
 
 -----BEGIN RSA PRIVATE KEY-----
BASE64 ENCODED DATA
-----END RSA PRIVATE KEY-----

 
  Within the base64 encoded data the following DER structure is present:
 
 RSAPrivateKey ::= SEQUENCE {
  version           Version,
  modulus           INTEGER,  -- n
  publicExponent    INTEGER,  -- e
  privateExponent   INTEGER,  -- d
  prime1            INTEGER,  -- p
  prime2            INTEGER,  -- q
  exponent1         INTEGER,  -- d mod (p-1)
  exponent2         INTEGER,  -- d mod (q-1)
  coefficient       INTEGER,  -- (inverse of q) mod p
  otherPrimeInfos   OtherPrimeInfos OPTIONAL
}

 
  Private Key file (PKCS#8)
 
 
  Because RSA is not used exclusively inside X509 and SSL/TLS, a more generic key format is available in the form of PKCS#8, that identifies the type of private key and contains the relevant data.
 
 
  The unencrypted PKCS#8 encoded data starts and ends with the tags:
 
 -----BEGIN PRIVATE KEY-----
BASE64 ENCODED DATA
-----END PRIVATE KEY-----

 
  Within the base64 encoded data the following DER structure is present:
 
 PrivateKeyInfo ::= SEQUENCE {
  version         Version,
  algorithm       AlgorithmIdentifier,
  PrivateKey      BIT STRING
}

AlgorithmIdentifier ::= SEQUENCE {
  algorithm       OBJECT IDENTIFIER,
  parameters      ANY DEFINED BY algorithm OPTIONAL
}

 
  So for an RSA private key, the OID is 1.2.840.113549.1.1.1 and there is a RSAPrivateKey as the PrivateKey key data bitstring.
 
 
  The encrypted PKCS#8 encoded data start and ends with the tags:
 
 -----BEGIN ENCRYPTED PRIVATE KEY-----
BASE64 ENCODED DATA
-----END ENCRYPTED PRIVATE KEY-----

 
  Within the base64 encoded data the following DER structure is present:
 
 EncryptedPrivateKeyInfo ::= SEQUENCE {
  encryptionAlgorithm  EncryptionAlgorithmIdentifier,
  encryptedData        EncryptedData
}

EncryptionAlgorithmIdentifier ::= AlgorithmIdentifier

EncryptedData ::= OCTET STRING

 
  The EncryptedData OCTET STRING is a PKCS#8 PrivateKeyInfo (see above).
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  https://polarssl.org/kb/cryptography/asn1-key-structures-in-der-and-pem
  
  
 


