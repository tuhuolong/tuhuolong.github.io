---
layout: post
title: RSA密钥文件(大整数的质数分解)  (公钥)modulus+publicExponent  (私钥)modulus+privateExponent
date: 2015-01-14 01:17:00
categories: [Tuhuolong]
tags: []
---

Private-Key: (31 bit)
modulus: 1911166693 (0x71ea16e5)
publicExponent: 65537 (0x10001)
privateExponent: 1068287013 (0x3facc425)
prime1: 53267 (0xd013)
prime2: 35879 (0x8c27)
exponent1: 37383 (0x9207)
exponent2: 19563 (0x4c6b)
coefficient: 6761 (0x1a69)
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
MCsCAQACBHHqFuUCAwEAAQIEP6zEJQIDANATAgMAjCcCAwCSBwICTGsCAhpp
-----END RSA PRIVATE KEY-----


modulus = prime1*prime2

publicExponent*privateExponent  mod (prime1-1)*(prime2-1) = 1



modulus+publicExponent (公钥)

modulus+privateExponent(私钥)
   

	-----BEGIN RSA PRIVATE KEY-----
	BASE64 ENCODED DATA
	-----END RSA PRIVATE KEY-----
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
	
###Private Key file (PKCS#8)
    
