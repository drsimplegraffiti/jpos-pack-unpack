
### Create a jpos server
Go to spring starter and create a spring boot project with no dependency

### Run
- add the jpos dependency in the pom.xml file.
```xml
 <dependencies>
        <dependency>
            <groupId>org.jpos</groupId>
            <artifactId>jpos</artifactId>
            <version>2.1.7</version>
        </dependency>
    </dependencies>
```

### Create a server
Method 1
```java
package com.drsimple.jposspringboot;

import org.jpos.q2.Q2;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class JposspringbootApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(JposspringbootApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        Q2 q2 = new Q2();
        Thread thread  = new Thread(q2);
        thread.start();
    }
}

```

Run: `mvn clean install`

Response:
```xml
<log realm="Q2.system" at="2025-05-03T07:51:35.351053200">
  <info>
    Q2 started, deployDir=C:\Users\CAPTAIN ENJOY\Downloads\jposspringboot\deploy, environment=default
  </info>
</log>
<log realm="Q2.system" at="2025-05-03T07:51:35.869952200" lifespan="515ms">
  <version>
    jPOS 2.1.7 master/7079725 (2022-01-22 18:50:13 UYT) 

-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA1

jPOS Community Edition, licensed under GNU AGPL v3.0.
This software is probably not suitable for commercial use.
Please see http://jpos.org/license for details.

-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1.4.9 (Darwin)

iQEcBAEBAgAGBQJMolHDAAoJEOQyeO71nYtFv74H/3OgehDGEy1VXp2U3/GcAobg
HH2eZjPUz53r38ARPiU3pzm9LwDa3WZgJJaa/b9VrJwKvbPwe9+0kY3gScDE1skT
ladHt+KHHmGQArEutkzHlpZa73RbroFEIa1qmN6MaDEHGoxZqDh0Sv2cpvOaVYGO
St8ZaddLBPC17bSjAPWo9sWbvL7FgPFOHhnPmbeux8SLtnfWxXWsgo5hLBanKmO1
1z+I/w/6DL6ZYZU6bAJUk+eyVVImJqw0x3IEElI07Nh9MC6BA4iJ77ejobj8HI2r
q9ulRPEqH9NR79619lNKVUkE206dVlXo7xHmJS1QZy5v/GT66xBxyDVfTduPFXk=
=oP+v
-----END PGP SIGNATURE-----

  </version>
</log>

```

### Method 2
```java
package com.drsimple.jposspringboot;

import org.jpos.q2.Q2;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;


@SpringBootApplication
public class JposspringbootApplication implements CommandLineRunner {

    public static void main(String[] args) {
        SpringApplication.run(JposspringbootApplication.class, args);
    }

    @Override
    public void run(String... args) {
        System.out.println("Starting jPOS Q2 server...");
        new Q2().start();  // This reads configuration from src/main/resources/deploy
    }

}
```

### Create a deploy folder in the resources folder
- create a file called server.xml
```xml
<server class="org.jpos.iso.ISOServer">
    <port>8000</port>
    <logger name="Q2" />
    <packager class="org.jpos.iso.packager.GenericPackager">
        <param name="packager-config" value="basic.xml" />
    </packager>
    <threadPool min="5" max="100" />
    <in>mti-logger</in>
</server>

<channel-adaptor name="mti-logger">
<class>org.jpos.util.LoggerAdaptor</class>
<logger name="Q2" />
</channel-adaptor>
```

and in the resources folder create a file called basic.xml
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE isopackager PUBLIC
        "-//jPOS/jPOS Generic Packager DTD 1.0//EN"
        "http://jpos.org/dtd/generic-packager-1.0.dtd">
<!-- ISO 8583:1993 (ASCII) field descriptions for GenericPackager -->
<isopackager headerLength="7">
    <isofield id="0" length="4" name="MESSAGE TYPE INDICATOR" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="1" length="16" name="BIT MAP" class="org.jpos.iso.IFA_BITMAP"/>
    <isofield id="2" length="19" name="PRIMARY ACCOUNT NUMBER" class="org.jpos.iso.IFA_LLNUM"/>
    <isofield id="3" length="6" name="PROCESSING CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="4" length="12" name="TRANSACTION AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="5" length="12" name="SETTLEMENT AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="6" length="12" name="CARDHOLDER BILLING AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="7" length="10" name="TRANSMISSION DATE AND TIME" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="8" length="8" name="CARDHOLDER BILLING FEE AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="9" length="8" name="SETTLEMENT CONVERSION RATE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="10" length="8" name="CARDHOLDER BILLING CONVERSION RATE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="11" length="6" name="SYSTEM TRACE AUDIT NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="12" length="6" name="LOCAL TRANSACTION TIME" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="13" length="4" name="LOCAL TRANSACTION DATE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="14" length="4" name="EXPIRATION DATE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="15" length="4" name="SETTLEMENT DATE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="16" length="4" name="CONVERSION DATE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="17" length="4" name="CAPTURE DATE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="18" length="4" name="MERCHANTS TYPE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="19" length="3" name="ACQUIRING INSTITUTION COUNTRY CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="20" length="3" name="PAN EXTENDED COUNTRY CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="21" length="3" name="FORWARDING INSTITUTION COUNTRY CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="22" length="3" name="POINT OF SERVICE ENTRY MODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="23" length="3" name="CARD SEQUENCE NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="24" length="3" name="NETWORK INTERNATIONAL IDENTIFIER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="25" length="2" name="POINT OF SERVICE CONDITION CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="26" length="2" name="POINT OF SERVICE PIN CAPTURE CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="27" length="1" name="AUTHORIZATION IDENTIFICATION RESP LEN" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="28" length="9" name="TRANSACTION FEE AMOUNT" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="29" length="9" name="SETTLEMENT FEE AMOUNT" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="30" length="9" name="TRANSACTION PROCESSING FEE AMOUNT" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="31" length="9" name="SETTLEMENT PROCESSING FEE AMOUNT" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="32" length="11" name="ACQUIRING INSTITUTION ID CODE" class="org.jpos.iso.IFA_LLNUM"/>
    <isofield id="33" length="11" name="FORWARDING INSTITUTION ID CODE" class="org.jpos.iso.IFA_LLNUM"/>
    <isofield id="34" length="28" name="PAN EXTENDED" class="org.jpos.iso.IFA_LLCHAR"/>
    <isofield id="35" length="37" name="TRACK 2 DATA" class="org.jpos.iso.IFA_LLNUM"/>
    <isofield id="36" length="104" name="TRACK 3 DATA" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="37" length="12" name="RETRIEVAL REFERENCE NUMBER" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="38" length="6" name="AUTHORIZATION IDENTIFICATION RESPONSE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="39" length="2" name="RESPONSE CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="40" length="3" name="SERVICE RESTRICTION CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="41" length="8" name="CARD ACCEPTOR TERMINAL IDENTIFICATION" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="42" length="15" name="CARD ACCEPTOR IDENTIFICATION CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="43" length="40" name="CARD ACCEPTOR NAME/LOCATION" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="44" length="25" name="ADDITIONAL RESPONSE DATA" class="org.jpos.iso.IFA_LLCHAR"/>
    <isofield id="45" length="76" name="TRACK 1 DATA" class="org.jpos.iso.IFA_LLCHAR"/>
    <isofield id="46" length="999" name="ADDITIONAL DATA - ISO" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="47" length="999" name="ADDITIONAL DATA - NATIONAL" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="48" length="999" name="ADDITIONAL DATA - PRIVATE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="49" length="3" name="TRANSACTION CURRENCY CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="50" length="3" name="SETTLEMENT CURRENCY CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="51" length="3" name="CARDHOLDER BILLING CURRENCY CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="52" length="16" name="PIN kDATA" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="53" length="16" name="SECURITY RELATED CONTROL INFORMATION" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="54" length="120" name="ADDITIONAL AMOUNTS" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="55" length="999" name="RESERVED ISO" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="56" length="999" name="RESERVED ISO" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="57" length="999" name="RESERVED NATIONAL" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="58" length="999" name="RESERVED NATIONAL" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="59" length="999" name="RESERVED NATIONAL" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="60" length="999" name="RESERVED PRIVATE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="61" length="999" name="RESERVED PRIVATE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="62" length="999" name="RESERVED PRIVATE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="63" length="999" name="RESERVED PRIVATE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="64" length="8" name="MESSAGE AUTHENTICATION CODE FIELD" class="org.jpos.iso.IFA_BINARY"/>
    <isofield id="65" length="1" name="BITMAP, EXTENDED" class="org.jpos.iso.IFA_BINARY"/>
    <isofield id="66" length="1" name="SETTLEMENT CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="67" length="2" name="EXTENDED PAYMENT CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="68" length="3" name="RECEIVING INSTITUTION COUNTRY CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="69" length="3" name="SETTLEMENT INSTITUTION COUNTRY CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="70" length="3" name="NETWORK MANAGEMENT INFORMATION CODE" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="71" length="4" name="MESSAGE NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="72" length="4" name="MESSAGE NUMBER LAST" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="73" length="6" name="DATE ACTION" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="74" length="10" name="CREDITS NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="75" length="10" name="CREDITS REVERSAL NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="76" length="10" name="DEBITS NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="77" length="10" name="DEBITS REVERSAL NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="78" length="10" name="TRANSFER NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="79" length="10" name="TRANSFER REVERSAL NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="80" length="10" name="INQUIRIES NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="81" length="10" name="AUTHORIZATION NUMBER" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="82" length="12" name="CREDITS, PROCESSING FEE AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="83" length="12" name="CREDITS, TRANSACTION FEE AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="84" length="12" name="DEBITS, PROCESSING FEE AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="85" length="12" name="DEBITS, TRANSACTION FEE AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="86" length="16" name="CREDITS, AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="87" length="16" name="CREDITS, REVERSAL AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="88" length="16" name="DEBITS, AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="89" length="16" name="DEBITS, REVERSAL AMOUNT" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="90" length="42" name="ORIGINAL DATA ELEMENTS" class="org.jpos.iso.IFA_NUMERIC"/>
    <isofield id="91" length="1" name="FILE UPDATE CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="92" length="2" name="FILE SECURITY CODE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="93" length="6" name="RESPONSE INDICATOR" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="94" length="7" name="SERVICE INDICATOR" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="95" length="42" name="REPLACEMENT AMOUNTS" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="96" length="16" name="MESSAGE SECURITY CODE" class="org.jpos.iso.IFA_BINARY"/>
    <isofield id="97" length="17" name="AMOUNT, NET SETTLEMENT" class="org.jpos.iso.IFA_AMOUNT"/>
    <isofield id="98" length="25" name="PAYEE" class="org.jpos.iso.IF_CHAR"/>
    <isofield id="99" length="11" name="SETTLEMENT INSTITUTION IDENT CODE" class="org.jpos.iso.IFA_LLNUM"/>
    <isofield id="100" length="11" name="RECEIVING INSTITUTION IDENT CODE" class="org.jpos.iso.IFA_LLNUM"/>
    <isofield id="101" length="17" name="FILE NAME" class="org.jpos.iso.IFA_LLCHAR"/>
    <isofield id="102" length="28" name="FROM ACCOUNT" class="org.jpos.iso.IFA_LLCHAR"/>
    <isofield id="103" length="10" name="ACCOUNT IDENTIFICATION 2" class="org.jpos.iso.IFA_LLCHAR"/>
    <isofield id="104" length="100" name="TRANSACTION DESCRIPTION" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="105" length="999" name="RESERVED ISO USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="106" length="999" name="RESERVED ISO USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="107" length="999" name="RESERVED ISO USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="108" length="999" name="RESERVED ISO USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="109" length="999" name="RESERVED ISO USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="110" length="999" name="RESERVED ISO USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="111" length="999" name="RESERVED ISO USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="112" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="113" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="114" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="115" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="116" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="117" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="118" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="119" length="999" name="RESERVED NATIONAL USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="120" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="121" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="122" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="123" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="124" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="125" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="126" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="127" length="999" name="RESERVED PRIVATE USE" class="org.jpos.iso.IFA_LLLCHAR"/>
    <isofield id="128" length="8" name="MAC 2" class="org.jpos.iso.IFA_BINARY"/>
</isopackager>
```

### Create a controller
```java
package com.drsimple.jposspringboot.controller;

import org.jpos.iso.ISOException;
import org.jpos.iso.ISOMsg;
import org.jpos.iso.ISOPackager;
import org.jpos.iso.packager.GenericPackager;
import org.jpos.iso.channel.ASCIIChannel;
import org.springframework.web.bind.annotation.*;

import java.io.IOException;
import java.net.Socket;

@RestController
@RequestMapping("/api/iso")
public class ISOMessageController {

    @PostMapping("/send")
    public String sendISOMessage(@RequestParam String pan,
                                 @RequestParam String amount,
                                 @RequestParam String processingCode) {

        try {
            // Load the packager config from classpath
            ISOPackager packager = new GenericPackager("basic.xml");

            // Create and populate the ISO message
            ISOMsg isoMsg = new ISOMsg();
            isoMsg.setPackager(packager);
            isoMsg.setMTI("0200");
            isoMsg.set(2, pan);              // Primary Account Number
            isoMsg.set(3, processingCode);   // Processing Code
            isoMsg.set(4, amount);           // Transaction Amount
            isoMsg.set(7, "1107221800");     // Transmission Date & Time (MMDDhhmmss)
            isoMsg.set(11, "123456");        // STAN
            isoMsg.set(41, "TERMID01");      // Terminal ID
            isoMsg.set(49, "840");           // Currency Code

            // Send via ASCIIChannel (connect to your jPOS server on port 8000)
            ASCIIChannel channel = new ASCIIChannel("localhost", 8000, packager);
            channel.connect();
            channel.send(isoMsg);

            // Receive response
            ISOMsg response = channel.receive();
            return "Response MTI: " + response.getMTI() + ", Field 39: " + response.getString(39);

        } catch (ISOException | IOException e) {
            e.printStackTrace();
            return "Failed to send ISO8583 message: " + e.getMessage();
        }
    }
}
```

