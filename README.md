# Tshark-Real-World-Investigation-Project
""An alert has been triggered: "A user came across a poor file index, and their curiosity led to problems". The case was assigned to you. Inspect the provided directory-curiosity.pcap. and retrieve the artefacts to confirm that this alert is a true positive, and retrieve the artefacts to confirm that this alert is a true positive."" I am acting as the SOC Analyst and I am tasked with investigating a triggered alert and see if it is a false or correct alert.

# Tools & Utilities Used
- Tshark
- VirusTotal

# Question 1: What is the name of the malicious/suspicious domain?
- I began by checking to see all the domains that have been accessed. So I use the below filter to see them.
- ![Capture](https://github.com/abdulhaire567/Tshark-Real-World-Investigation-Project/blob/main/Screenshot%202025-04-17%20151039.png)

- After seeing all the domain names listed we put each one of them in virustotal to see which one is a malicious domain. After trying all the domains jx2-bavuong[.]com stood out. So I have to investigate it more and see more details about it.

# Question 2: What is the total number of HTTP requests sent to the malicious domain?
- I had to find the number of http requests sent to this domain so I used this filter (tshark -r directory-curiosity.pcap -T fields -e dns.qryname | awk NF | sort -r | uniq -c | sort -r), to see exactly how many was sent.
 - ![Capture](https://github.com/abdulhaire567/Tshark-Real-World-Investigation-Project/blob/main/Screenshot%202025-04-17%20151353.png)

# Question 3: What is the IP address associated with the malicious domain?
- To find the IP Address associated with our malicious domain we used (tshark -r directory-curiosity.pcap -T fields -Y "dns.qry.name == \"jx2-bavuong.com\"" -T fields -e dns.a).
- -r directory-curiosity.pcap — Read the capture file named directory-curiosity.pcap.

- -T fields — Output only specific fields (instead of the whole packet details).

- -Y "dns.qry.name == \"jx2-bavuong.com\"" — Apply a display filter: only show packets where the DNS query name is exactly jx2-bavuong.com.

- -e dns.a — Extract the dns.a field, which is the IPv4 address from a DNS response
- (![Capture](https://github.com/abdulhaire567/Tshark-Real-World-Investigation-Project/blob/main/Screenshot%202025-04-17%20151353.png)

# Question 4: What is the server info of the suspicious domain?
- To find the the server associated with the suspicious domain. We use (tshark -r directory-curiosity.pcap -Y "http.server" -T fields -e ip.dst -e http.server)
- -Y "http.server" — Display filter: find packets that have an http.server field (basically HTTP responses that include the server's name/type — like "Apache", "nginx", etc.).

- -T fields — Format output to show only selected fields.

- -e ip.dst — Extract the destination IP address (where the HTTP server response is sent).

- -e http.server — Extract the http.server header field (reveals what software the server is running).
- ![Capture](https://github.com/abdulhaire567/Tshark-Real-World-Investigation-Project/blob/main/Screenshot%202025-04-17%20152836.png)

# Question 5: Follow the "first TCP stream" in "ASCII". Investigate the output carefully. What is the number of listed files?
- For us to follow the first TCP stream in ascii to see how many number of files it contains. We use (tshark -r directory-curiosity.pcap -z follow,tcp,ascii,0 -q). After investigation I found three files.
- -z follow,tcp,ascii,0 → Follow TCP stream 0 and show its ASCII text (readable data).

- -q → Quiet mode — hide normal packet output, show only the stream data.
- ![Capture](https://github.com/abdulhaire567/Tshark-Real-World-Investigation-Project/blob/main/Screenshot%202025-04-17%20154835.png)

