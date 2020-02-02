---
title: Comparison of DSA and RSA
markup: "mmark"
weight: 2
---

## Comparison of DSA and RSA

|                	| RSA                                                          	| DSA                                                   	|
|----------------	|--------------------------------------------------------------	|-------------------------------------------------------	|
| Security       	| Based on factoring large numbers                             	| Based on taking discrete logarithms                   	|
| Functionality  	| Encryption and Signatures                                    	| Signatures only                                       	|
| Performance    	| Faster than DSA in verification About the same in generation 	| Some signature computation can be computed a priori   	|
| Exposure       	| Message digest can be recovered from the signature           	| Message digest cannot be recovered from the signature 	|
| Ephermeral Key 	| No ephemeral key required                                    	| One for each message                                  	|
