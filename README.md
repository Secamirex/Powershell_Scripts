# PowerShell encoded script investigation

The following suspicious PowerShell-encoded script has been received.  Let's look into the contents of the script and try to unveil its true nature and potential risks.


![image](https://github.com/user-attachments/assets/d8240950-709c-45fa-b6bf-35d57e950638)

# Investigation steps:

1. The file seems to be encoded using Base 64 format.
2. Head over to CyberChef, select  "from Base64" and paste the encoded value in the input box.
3. Add the " Remove null byte " option to eliminate  all Null values.
4. Check the output to determine the script's intention. 


![image](https://github.com/user-attachments/assets/15875dd7-bddd-4b00-b438-c9efe2b7e997)

# Observations:

* The file creates a WebcClient object  : $WC New-Object SySTeM.NET.WebCliENt
* Naming has been obfuscated by mixing uppercase and lowercase letters 
* Sets the User-Agent: $u Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; rv:11.0) like Gecko' $WC.HeADeRS.ADd('User-Agent', $u)
* Configure a proxy and sets the credentials: <br> 
	$Wc.ProxY  [System.NeT.WEBReQUEst]::DEFAuLtWebProXy
	$wc.PROxY.CrEdenTialS  [SysTem.NEt.CRedeNTIAlCAcHE]::DeFAULTNetWOrKCredENTiAls
	
* Performs obfuscation of key :  $K = 'IM-S&fA9Xu{[)|wdWJhC+!N~vq_12Lty'
* The script downloads content from http[:]//98[.]103[.]103[.]170[:]7443/index.asp, likely a remote server .
* The result of the operation is stored in $B variable. 
* It uses IEX ( Invoke-Expression) to execute the contents of $B variable, which suggests it tries to execute the code received from remote location.

# Investigation Summary: 
	
* The script is likely downloading a piece of code from a remote server, decrypting it using XOR with a key, and then executing it.
* The downloaded content might be malicious in nature, and the use of Invoke-Expression to execute it further suggests that it is trying to run dynamic code received from a remote location.
* The  system this script was found on needs to be investigated further for data exfiltration or attempted attack.
