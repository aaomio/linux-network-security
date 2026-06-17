###########################
# Snort Rules
###########################

#1 External Host Detection
alert tcp !$HOME_NET any -> any any (msg:"External Host Detected"; sid:10000001; priority:2;)

#2 Anonymized IP Detected
alert ip [10.0.0.0/8] any -> $HOME_NET any (msg:"Anonymized External IP Detected"; sid:10000002; priority:3;)

#3 Privilege Escalation Detection
alert tcp any any -> $HOME_NET 80 (msg:"CVE-2023-20198: Privilege Escalation (Priv-Level: 15) Detected"; sid:10000003; priority:1;)

#4 Double Encoding Detection
alert tcp any any -> $HOME_NET 80 (msg:"Double Encoding Attack Detected"; content:"%2577ebui_wsma_Http"; sid:10000004; priority:2;)

#5 Detects unauthorized username creation
alert tcp any any -> $HOME_NET 80 (msg:"CVE-2023-20198: Unauthorized Username Creation Detected"; sid:10000005; priority:2;)

#6 Detects execution of "show ver" command
alert tcp any any -> $HOME_NET 80 (msg:"Unauthorized Command Execution: 'show ver' Detected"; sid:10000006; priority:3;)

#7 Large POST Request Detection
alert tcp any any -> $HOME_NET 80 (msg:"Large HTTP POST Detected"; content:"POST"; http_method; sid:10000007; priority:3;)

#8 Excess HTTP Data Detection
alert tcp any any -> $HOME_NET 80 (msg:"CVE-2023-20198: Malicious XML Payload"; flow:to_server,established; content:"Extra data after"; sid:10000008; priority:2;)

#10 Internal User Detected
alert tcp [192.168.1.0/24] any -> $HOME_NET any (msg:"IP Range Detected - TCP Traffic"; sid:10000009; priority:5;)

#11 HTTP POST Missing User-Agent
alert tcp any any -> $HOME_NET 80 (msg:"HTTP POST Missing User-Agent Header"; content:"POST"; sid:10000010; priority:4;)

###########################
# Packet 1
###########################

echo -e "POST /%2577ebui_wsma Http HTTP/1.1\r\n" \
"Host: 192.168.1.1\r\n" \
"Priv-Level: 15\r\n" \
"User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)\r\n" \
"Content-Length: 720\r\n\r\n\r\n" \
"<xml version=\"1.0\"><SOAP:Envelope><SOAP:Body>" \
"<cli-config-data-block>" \
"username cisco password privilege 15 password secret123" \
"</cli-config-data-block></SOAP:Body></SOAP:Envelope>" > pcap1_payload

###########################
# Packet 2
###########################

echo -e "POST %2577ebui_wsma_Http HTTP/1.1\r\n\r\n" \
"Host: 192.168.1.1\r\n\r\n" \
"User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)\r\n" \
"Content-Length: 900\r\n\r\n" \
"<SOAP:Body><SOAP:Envelope><SOAP:Body>" \
"<show><sent>show ver</sent></dialogueLog></execLog>" > pcap2_payload
