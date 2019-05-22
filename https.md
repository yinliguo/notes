## HTTPS
### 生成证书
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ssl.key -out ssl.crt
; Common name输入正确的域名
openssl dhparam -out dhparam.pem 2048
```
```
# nginx
server {
	listen      443 ssl;
	server_name localhost;
	
	ssl_certificate ssl.crt;
	ssl_certificate_key ssl.key;
	ssl_dhparam dhparam.pem
	ssl_session_cache    shared:SSL:1m;
	ssl_session_timeout  5m;
	ssl_ciphers  HIGH:!aNULL:!MD5;
	ssl_prefer_server_ciphers  on;
	
	location / {
		root  /www;
		index index.html index.htm;
	}
}
```
### SSL握手
* 基于TCP握手后才能进行SSL握手

#### 1、Client -> Server: Client Hello
```
Transport Layer Security
    TLSv1.2 Record Layer: Handshake Protocol: Client Hello
        Content Type: Handshake (22)
        Version: TLS 1.0 (0x0301)
        Length: 512
        Handshake Protocol: Client Hello
            Handshake Type: Client Hello (1)
            Length: 508
            // 客户端支持的最高版本
            Version: TLS 1.2 (0x0303)
            // 随机数randomX
            Random: 778a23983d70d009fc0c504f41ed0bf8c1d9ca20573370ae…
                GMT Unix Time: Jul 21, 2033 14:56:24.000000000 CST
                Random Bytes: 3d70d009fc0c504f41ed0bf8c1d9ca20573370ae15d3f669…
            Session ID Length: 32
            // 如果客户端想要恢复握手，只需要发送Session ID
            Session ID: 2545ea833760b9c13d3bc961ddfa27cf4dfb08fbde037b70…
            Cipher Suites Length: 52
            // 客户端支持的加密套件
            Cipher Suites (26 suites)
                Cipher Suite: TLS_CHACHA20_POLY1305_SHA256 (0x1303)
                Cipher Suite: TLS_AES_128_GCM_SHA256 (0x1301)
                Cipher Suite: TLS_AES_256_GCM_SHA384 (0x1302)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 (0xc02c)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 (0xc02b)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 (0xc024)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 (0xc023)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (0xc00a)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (0xc009)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256 (0xcca9)
                Cipher Suite: TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (0xc030)
                Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (0xc02f)
                Cipher Suite: TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (0xc028)
                Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (0xc027)
                Cipher Suite: TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (0xc014)
                Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (0xc013)
                Cipher Suite: TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (0xcca8)
                Cipher Suite: TLS_RSA_WITH_AES_256_GCM_SHA384 (0x009d)
                Cipher Suite: TLS_RSA_WITH_AES_128_GCM_SHA256 (0x009c)
                Cipher Suite: TLS_RSA_WITH_AES_256_CBC_SHA256 (0x003d)
                Cipher Suite: TLS_RSA_WITH_AES_128_CBC_SHA256 (0x003c)
                Cipher Suite: TLS_RSA_WITH_AES_256_CBC_SHA (0x0035)
                Cipher Suite: TLS_RSA_WITH_AES_128_CBC_SHA (0x002f)
                Cipher Suite: TLS_ECDHE_ECDSA_WITH_3DES_EDE_CBC_SHA (0xc008)
                Cipher Suite: TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA (0xc012)
                Cipher Suite: TLS_RSA_WITH_3DES_EDE_CBC_SHA (0x000a)
            Compression Methods Length: 1
            Compression Methods (1 method)
                Compression Method: null (0)
            Extensions Length: 383
            Extension: renegotiation_info (len=1)
                Type: renegotiation_info (65281)
                Length: 1
                Renegotiation Info extension
                    Renegotiation info extension length: 0
            Extension: server_name (len=20)
                Type: server_name (0)
                Length: 20
                Server Name Indication extension
                    Server Name list length: 18
                    Server Name Type: host_name (0)
                    Server Name length: 15
                    Server Name: 192.168.199.220
            Extension: extended_master_secret (len=0)
                Type: extended_master_secret (23)
                Length: 0
            Extension: signature_algorithms (len=24)
                Type: signature_algorithms (13)
                Length: 24
                Signature Hash Algorithms Length: 22
                Signature Hash Algorithms (11 algorithms)
                    Signature Algorithm: ecdsa_secp256r1_sha256 (0x0403)
                        Signature Hash Algorithm Hash: SHA256 (4)
                        Signature Hash Algorithm Signature: ECDSA (3)
                    Signature Algorithm: rsa_pss_rsae_sha256 (0x0804)
                        Signature Hash Algorithm Hash: Unknown (8)
                        Signature Hash Algorithm Signature: Unknown (4)
                    Signature Algorithm: rsa_pkcs1_sha256 (0x0401)
                        Signature Hash Algorithm Hash: SHA256 (4)
                        Signature Hash Algorithm Signature: RSA (1)
                    Signature Algorithm: ecdsa_secp384r1_sha384 (0x0503)
                        Signature Hash Algorithm Hash: SHA384 (5)
                        Signature Hash Algorithm Signature: ECDSA (3)
                    Signature Algorithm: ecdsa_sha1 (0x0203)
                        Signature Hash Algorithm Hash: SHA1 (2)
                        Signature Hash Algorithm Signature: ECDSA (3)
                    Signature Algorithm: rsa_pss_rsae_sha384 (0x0805)
                        Signature Hash Algorithm Hash: Unknown (8)
                        Signature Hash Algorithm Signature: Unknown (5)
                    Signature Algorithm: rsa_pss_rsae_sha384 (0x0805)
                        Signature Hash Algorithm Hash: Unknown (8)
                        Signature Hash Algorithm Signature: Unknown (5)
                    Signature Algorithm: rsa_pkcs1_sha384 (0x0501)
                        Signature Hash Algorithm Hash: SHA384 (5)
                        Signature Hash Algorithm Signature: RSA (1)
                    Signature Algorithm: rsa_pss_rsae_sha512 (0x0806)
                        Signature Hash Algorithm Hash: Unknown (8)
                        Signature Hash Algorithm Signature: Unknown (6)
                    Signature Algorithm: rsa_pkcs1_sha512 (0x0601)
                        Signature Hash Algorithm Hash: SHA512 (6)
                        Signature Hash Algorithm Signature: RSA (1)
                    Signature Algorithm: rsa_pkcs1_sha1 (0x0201)
                        Signature Hash Algorithm Hash: SHA1 (2)
                        Signature Hash Algorithm Signature: RSA (1)
            Extension: status_request (len=5)
                Type: status_request (5)
                Length: 5
                Certificate Status Type: OCSP (1)
                Responder ID list Length: 0
                Request Extensions Length: 0
            Extension: next_protocol_negotiation (len=0)
                Type: next_protocol_negotiation (13172)
                Length: 0
            Extension: signed_certificate_timestamp (len=0)
                Type: signed_certificate_timestamp (18)
                Length: 0
            Extension: application_layer_protocol_negotiation (len=48)
                Type: application_layer_protocol_negotiation (16)
                Length: 48
                ALPN Extension Length: 46
                ALPN Protocol
                    ALPN string length: 2
                    ALPN Next Protocol: h2
                    ALPN string length: 5
                    ALPN Next Protocol: h2-16
                    ALPN string length: 5
                    ALPN Next Protocol: h2-15
                    ALPN string length: 5
                    ALPN Next Protocol: h2-14
                    ALPN string length: 8
                    ALPN Next Protocol: spdy/3.1
                    ALPN string length: 6
                    ALPN Next Protocol: spdy/3
                    ALPN string length: 8
                    ALPN Next Protocol: http/1.1
            Extension: ec_point_formats (len=2)
                Type: ec_point_formats (11)
                Length: 2
                EC point formats Length: 1
                Elliptic curves point formats (1)
                    EC point format: uncompressed (0)
            Extension: key_share (len=38)
                Type: key_share (51)
                Length: 38
                Key Share extension
                    Client Key Share Length: 36
                    Key Share Entry: Group: x25519, Key Exchange length: 32
                        Group: x25519 (29)
                        Key Exchange Length: 32
                        Key Exchange: 4aa981477c91bfef8031fe357756e403ef87261cfec47b4a…
            Extension: psk_key_exchange_modes (len=2)
                Type: psk_key_exchange_modes (45)
                Length: 2
                PSK Key Exchange Modes Length: 1
                PSK Key Exchange Mode: PSK with (EC)DHE key establishment (psk_dhe_ke) (1)
            Extension: supported_versions (len=9)
                Type: supported_versions (43)
                Length: 9
                Supported Versions length: 8
                Supported Version: TLS 1.3 (0x0304)
                Supported Version: TLS 1.2 (0x0303)
                Supported Version: TLS 1.1 (0x0302)
                Supported Version: TLS 1.0 (0x0301)
            Extension: supported_groups (len=10)
                Type: supported_groups (10)
                Length: 10
                Supported Groups List Length: 8
                Supported Groups (4 groups)
                    Supported Group: x25519 (0x001d)
                    Supported Group: secp256r1 (0x0017)
                    Supported Group: secp384r1 (0x0018)
                    Supported Group: secp521r1 (0x0019)
            Extension: padding (len=168)
                Type: padding (21)
                Length: 168
                Padding Data: 000000000000000000000000000000000000000000000000…

```
#### 2、Server -> Client: Server Hello
> 在发送之前先回复ACK

```
Transport Layer Security
    TLSv1.2 Record Layer: Handshake Protocol: Server Hello
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 108
        Handshake Protocol: Server Hello
            Handshake Type: Server Hello (2)
            Length: 104
            // 选择的协议版本，应该是双方都支持的最高的版本
            Version: TLS 1.2 (0x0303)
            // 随机数randomY
            Random: ac89fb6c5d775f6bb3b01201d81dfc632c645483708b96b0…
                GMT Unix Time: Sep 24, 2061 01:59:08.000000000 CST
                Random Bytes: 5d775f6bb3b01201d81dfc632c645483708b96b002f88568…
            Session ID Length: 32
            // 确认/允许恢复发送的Session ID
            Session ID: 0e7fd3db344179ba8e10002259c09f733640e85e8a849199…
            // 选择的加密套件
            Cipher Suite: TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (0xc030)
            // 选择的压缩方法
            Compression Method: null (0)
            Extensions Length: 32
            Extension: server_name (len=0)
                Type: server_name (0)
                Length: 0
            Extension: renegotiation_info (len=1)
                Type: renegotiation_info (65281)
                Length: 1
                Renegotiation Info extension
                    Renegotiation info extension length: 0
            Extension: ec_point_formats (len=4)
                Type: ec_point_formats (11)
                Length: 4
                EC point formats Length: 3
                Elliptic curves point formats (3)
                    EC point format: uncompressed (0)
                    EC point format: ansiX962_compressed_prime (1)
                    EC point format: ansiX962_compressed_char2 (2)
            Extension: application_layer_protocol_negotiation (len=11)
                Type: application_layer_protocol_negotiation (16)
                Length: 11
                ALPN Extension Length: 9
                ALPN Protocol
                    ALPN string length: 8
                    ALPN Next Protocol: http/1.1
    // 证书信息
    TLSv1.2 Record Layer: Handshake Protocol: Certificate
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 926
        Handshake Protocol: Certificate
            Handshake Type: Certificate (11)
            Length: 922
            Certificates Length: 919
            Certificates (919 bytes)
                Certificate Length: 916
                Certificate: 3082039030820278020900b670074adba3e407300d06092a… (pkcs-9-at-emailAddress=yinlg@outlook.com,id-at-commonName=localhost,id-at-organizationalUnitName=noname,id-at-organizationName=noname,id-at-localityName=BeiJing,id-at-stateO
                    signedCertificate
                        serialNumber: 13146015330388403207
                        signature (sha256WithRSAEncryption)
                            Algorithm Id: 1.2.840.113549.1.1.11 (sha256WithRSAEncryption)
                        issuer: rdnSequence (0)
                            rdnSequence: 7 items (pkcs-9-at-emailAddress=yinlg@outlook.com,id-at-commonName=localhost,id-at-organizationalUnitName=noname,id-at-organizationName=noname,id-at-localityName=BeiJing,id-at-stateOrProvinceName=BeiJing,id-at-countryName=CN)
                                RDNSequence item: 1 item (id-at-countryName=CN)
                                    RelativeDistinguishedName item (id-at-countryName=CN)
                                        Id: 2.5.4.6 (id-at-countryName)
                                        CountryName: CN
                                RDNSequence item: 1 item (id-at-stateOrProvinceName=BeiJing)
                                    RelativeDistinguishedName item (id-at-stateOrProvinceName=BeiJing)
                                        Id: 2.5.4.8 (id-at-stateOrProvinceName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: BeiJing
                                RDNSequence item: 1 item (id-at-localityName=BeiJing)
                                    RelativeDistinguishedName item (id-at-localityName=BeiJing)
                                        Id: 2.5.4.7 (id-at-localityName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: BeiJing
                                RDNSequence item: 1 item (id-at-organizationName=noname)
                                    RelativeDistinguishedName item (id-at-organizationName=noname)
                                        Id: 2.5.4.10 (id-at-organizationName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: noname
                                RDNSequence item: 1 item (id-at-organizationalUnitName=noname)
                                    RelativeDistinguishedName item (id-at-organizationalUnitName=noname)
                                        Id: 2.5.4.11 (id-at-organizationalUnitName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: noname
                                RDNSequence item: 1 item (id-at-commonName=localhost)
                                    RelativeDistinguishedName item (id-at-commonName=localhost)
                                        Id: 2.5.4.3 (id-at-commonName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: localhost
                                RDNSequence item: 1 item (pkcs-9-at-emailAddress=yinlg@outlook.com)
                                    RelativeDistinguishedName item (pkcs-9-at-emailAddress=yinlg@outlook.com)
                                        Id: 1.2.840.113549.1.9.1 (pkcs-9-at-emailAddress)
                                        IA5String: yinlg@outlook.com
                        validity
                            notBefore: utcTime (0)
                                utcTime: 19-05-21 12:28:39 (UTC)
                            notAfter: utcTime (0)
                                utcTime: 20-05-20 12:28:39 (UTC)
                        subject: rdnSequence (0)
                            rdnSequence: 7 items (pkcs-9-at-emailAddress=yinlg@outlook.com,id-at-commonName=localhost,id-at-organizationalUnitName=noname,id-at-organizationName=noname,id-at-localityName=BeiJing,id-at-stateOrProvinceName=BeiJing,id-at-countryName=CN)
                                RDNSequence item: 1 item (id-at-countryName=CN)
                                    RelativeDistinguishedName item (id-at-countryName=CN)
                                        Id: 2.5.4.6 (id-at-countryName)
                                        CountryName: CN
                                RDNSequence item: 1 item (id-at-stateOrProvinceName=BeiJing)
                                    RelativeDistinguishedName item (id-at-stateOrProvinceName=BeiJing)
                                        Id: 2.5.4.8 (id-at-stateOrProvinceName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: BeiJing
                                RDNSequence item: 1 item (id-at-localityName=BeiJing)
                                    RelativeDistinguishedName item (id-at-localityName=BeiJing)
                                        Id: 2.5.4.7 (id-at-localityName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: BeiJing
                                RDNSequence item: 1 item (id-at-organizationName=noname)
                                    RelativeDistinguishedName item (id-at-organizationName=noname)
                                        Id: 2.5.4.10 (id-at-organizationName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: noname
                                RDNSequence item: 1 item (id-at-organizationalUnitName=noname)
                                    RelativeDistinguishedName item (id-at-organizationalUnitName=noname)
                                        Id: 2.5.4.11 (id-at-organizationalUnitName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: noname
                                RDNSequence item: 1 item (id-at-commonName=localhost)
                                    RelativeDistinguishedName item (id-at-commonName=localhost)
                                        Id: 2.5.4.3 (id-at-commonName)
                                        DirectoryString: uTF8String (4)
                                            uTF8String: localhost
                                RDNSequence item: 1 item (pkcs-9-at-emailAddress=yinlg@outlook.com)
                                    RelativeDistinguishedName item (pkcs-9-at-emailAddress=yinlg@outlook.com)
                                        Id: 1.2.840.113549.1.9.1 (pkcs-9-at-emailAddress)
                                        IA5String: yinlg@outlook.com
                        subjectPublicKeyInfo
                            algorithm (rsaEncryption)
                                Algorithm Id: 1.2.840.113549.1.1.1 (rsaEncryption)
                            subjectPublicKey: 3082010a0282010100c012374a0a62c4ad6858ff2b8f03df…
                                modulus: 0x00c012374a0a62c4ad6858ff2b8f03df82bfce5a2a4b5929…
                                publicExponent: 65537
                    algorithmIdentifier (sha256WithRSAEncryption)
                        Algorithm Id: 1.2.840.113549.1.1.11 (sha256WithRSAEncryption)
                    Padding: 0
                    encrypted: 18c94fd302a6209506a05d7755be88dddeab5605c05880f9…
    // 发送ServerKeyExchange消息（依赖所选择的加密套件）
    TLSv1.2 Record Layer: Handshake Protocol: Server Key Exchange
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 333
        Handshake Protocol: Server Key Exchange
            Handshake Type: Server Key Exchange (12)
            Length: 329
            EC Diffie-Hellman Server Params
                Curve Type: named_curve (0x03)
                Named Curve: secp256r1 (0x0017)
                Pubkey Length: 65
                Pubkey: 046634b2fd5318f2a25da36b0c5a5264692f8ae4a2dfe0c6…
                Signature Algorithm: rsa_pkcs1_sha256 (0x0401)
                    Signature Hash Algorithm Hash: SHA256 (4)
                    Signature Hash Algorithm Signature: RSA (1)
                Signature Length: 256
                Signature: 7b6075538b9064318fa813847c9016962fce45fb1f0b7c1c…
    // 标志协商完成
    TLSv1.2 Record Layer: Handshake Protocol: Server Hello Done
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 4
        Handshake Protocol: Server Hello Done
            Handshake Type: Server Hello Done (14)
            Length: 0

```
#### 3、Client -> Server: Client Key Exchange, Change Cipher Spec, Encrypted Handshake Message
> 在发送之前会先回复一个ACK

```
Transport Layer Security
    TLSv1.2 Record Layer: Handshake Protocol: Client Key Exchange
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 70
        Handshake Protocol: Client Key Exchange
            Handshake Type: Client Key Exchange (16)
            Length: 66
            EC Diffie-Hellman Client Params
                Pubkey Length: 65
                // 公钥
                Pubkey: 04643d8e7c3ecb1ace1d1bb6bbbd7f71ae51d814ca2b40bb…
    TLSv1.2 Record Layer: Change Cipher Spec Protocol: Change Cipher Spec
        Content Type: Change Cipher Spec (20)
        Version: TLS 1.2 (0x0303)
        Length: 1
        Change Cipher Spec Message
    TLSv1.2 Record Layer: Handshake Protocol: Encrypted Handshake Message
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 40
        Handshake Protocol: Encrypted Handshake Message

```
#### 4、Server -> Client: Change Cipher Spec, Encrypted Handshake Message
> 在发送之前先回复ACK

```
Transport Layer Security
    TLSv1.2 Record Layer: Change Cipher Spec Protocol: Change Cipher Spec
        Content Type: Change Cipher Spec (20)
        Version: TLS 1.2 (0x0303)
        Length: 1
        Change Cipher Spec Message
    TLSv1.2 Record Layer: Handshake Protocol: Encrypted Handshake Message
        Content Type: Handshake (22)
        Version: TLS 1.2 (0x0303)
        Length: 40
        Handshake Protocol: Encrypted Handshake Message

```