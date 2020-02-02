---
title: Comparison of Key Establishment Protocols
markup: "mmark"
weight: 6
---

## Comparison of Secret Key Establishment Protocols

| Protocol          	| Third Party  	| Timestamps 	| Entity Authentication 	| Messages 	|
|-------------------	|--------------	|------------	|-----------------------	|----------	|
| Diffie-Hellman    	| No           	| No         	| None                  	| 2        	|
| Needham-Schroeder 	| KDC (online) 	| No         	| Symmetric encryption  	| 5        	|
| Kerberos          	| KDC (online) 	| Yes        	| Symmetric encryption  	| 4        	|
| X.509 (2 Pass)    	| CA (offline) 	| Yes        	| Mutual                	| 2        	|
| X.509 (3 Pass)    	| CA (offline) 	| No, nonces 	| Mutual                	| 3        	|
