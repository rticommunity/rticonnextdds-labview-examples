# Example Code — Secure Shapes Demo

Applies to RTI DDS Toolkit for LabVIEW 1.5.0.95 and above.

## Example Description
This example shows how the RTI DDS Toolkit for LabVIEW works with Secure DDS.
Several scenarios using the ShapeType will be shown in this example. Shapes
Demo 5.2.4 can be used to graphically show this communication. You can ask for
a [Shapes Demo 5.2.4 trial.](https://www.rti.com/downloads/index.html#REGISTER)

We will use three topics:
- Square
- Circle
- Triangle

And three security configurations:
- AllowAll
- SecureDenyPubCircles
- SecureDenySubSquares

We will use these three security configurations to set up a secure environment
which uses [Secure DDS](https://www.rti.com/products/secure.html).
You can use this example as a base to create many other security 
configurations.
To do that, you will need to generate your own security certificates,
which is explained [Secure DDS Getting
Started Guide](https://www.google.com/url?q=https%3A%2F%2Fcommunity.rti.com%2Fstatic%2Fdocumentation%2Fconnext-dds%2F5.2.4%2Fdoc%2Fmanuals%2Fconnext_dds%2Fsecure_dds%2FRTI_SecureDDS_GettingStarted.pdf&sa=D&sntz=1&usg=AFQjCNG2GIr_fINO7Q8rXCAbeS-b3n8pvA).

These security configurations are provided in the file 
RTI_SHAPES_DEMO_PERMISSIONS.p7s. We will use these configuration to create 
the following Custom Secure Profiles: The security configurations that will
be created are:
- `AllowAll`: This configuration enables secure communication between
all the domain IDs and topics. The characteristics of this communication are
specified in the Governance. The permissions rules are:
``` xml
        <default>ALLOW</default>
```
- `SecureDenyPubCircles`: This configuration won’t allow you to publish to the
‘Circles’ topic. The permissions rules are:
``` xml
  <deny_rule>
    <domain_id>0</domain_id>
    <publish>
      <topic>Circle*</topic>
    </publish>
  </deny_rule>
  <default>ALLOW</default>
```
- `SecureDenySubSquares`: This configuration won't allow you to create 
DataReaders for the topic 'Square'. The permissions rules are:
``` xml
  <deny_rule>
    <domain_id>0</domain_id>
      <subscribe>
        <topic>Square*</topic>
      </subscribe>
  </deny_rule>
  <default>ALLOW</default>
```



## Description of VIs
The Secure Shapes Demo example is divided into  seven different VIs.
- `Get full path from file name.vi`: Auxiliary VI which returns a full path
that points to the file whose name is 'File Name' and is in a subfolder
called 'cert' in the same folder as the current VI.
- `Basic Security Configuration From Path.vi`: Creates a basic security
configuration which uses the AllowAll configuration. This subVI will take the
security certificates from a subfolder called 'cert' in the same directory as
the VI.
- `Create Secure Profile If Does Not Exist.vi`: Creates a Security 
configuration if the provided name does not exist.
- `Create SecureDenyPubCircles Profile.vi`: Adds the files 
RTI_SHAPES_DEMO_PEER_2 cert and key to the provided base Secure Profile. This 
configuration will use the permissions of SecureDenyPubCircles which denies 
publications to the topic 'Circle'.
- `Create SecureDenySubSquares Profile.vi`: Adds the files 
RTI_SHAPES_DEMO_PEER_3 cert and key to the provided base Secure Profile. This 
configuration will use the permissions of SecureDenySubSquares which denies 
subscriptions to the 'Square' topic.
- `RTI Connext DDS Secure Shapes Reader.vi`: Main Reader
application. This VI will create the DDS entities to subscribe to Shapes. It
will be created with the security configuration 'SecureDenySubSquares'.
- `RTI Connext DDS Secure Shapes Writer.vi`: Main Writer
application. This VI will create the DDS entities to publish 'Shapes'. It
will be created with the security configuration 'SecureDenyPubCircles'.


# Main Scenarios
There are three main scenarios (one per topic). All of them will be
explained with the default Secure Custom Profiles that this application uses.
These default Secure Custom Profiles are:
    - Writer: `SecureDenyPubCircles`.
    - Reader: `SecureDenySubSquares`.
1. Topic `Square`: If the Writer and Reader are created with the default Custom
Secure  Profile for this example, the Reader won't be able to subscribe
to the `Square` topic. Therefore, no communication will occur.
2. Topic `Circle`: If the Writer and Reader are created with the default 
Custom Secure Profile in the Topic `Circle`, the Writer won't send any sample 
because it is not allowed by the security permissions.
3. Topic `Triangle`: If the Writer and the Reader are created with the default
Custom Secure Profile in the Topic `Triangle`, the communication will be fine
because no restrictions apply to this topic.


## Running LabVIEW Example
1. Topic `Square`.
    - Run the Writer using the topic `Square` and the Secure Custom Profile
    `SecureDenyPubCircles` or `AllowAll`. By default, the Writer will be 
	created with `SecureDenyPubCircles`.
    - The Writer will start to publish a square. You can see it in Shapes Demo
    using the  `AllowAll` configuration.
    - Run the Reader with the topic 'Square' using the Secure Custom Profile
    `SecureDenySubSquares`.
    - You will receive an error because the Security permissions do not allow 
	you to create a DataReader in the topic `Square`.

2. Topic `Circle`.
    - Run the Reader using the topic `Circle` with the Secure Custom Profile
    `AllowAll` or `SecureDenySubSquares`. By default, the Reader will be 
	created with `SecureDenySubSquares`.
    - Run the Writer in the topic `Circle` with the Secure Custom Profile
    `SecureDenyPubCircles`, which is the default.
    - No communication will occur because the Writer cannot use the topic
    `Circle`.

3. Topic `Triangle`:
    - Run the Writer using the topic `Triangle` with the Secure Custom Profile 
	`AllowAll` or `SecureDenyPubCircles`. By default, the Writer will be 
	created with `SecureDenyPubCircles`.
    - Run the Reader using the topic `Triangle` with the Secure Custom 
	Profile `AllowAll` or `SecureDenySubSquares`. By default, the Reader
	will be created with `SecureDenySubSquares`.
    - Since there are no restrictions on the `Triangle` topic, the 
	communication will work fine.

4. Test your own scenario. For instance:
    - Try to use RTI_SHAPES_DEMO_PEER_2_CERT.pem with
    RTI_SHAPES_DEMO_PEER_3_KEY.pem in the same Custom Secure Profile (no
    communication will occur).
    - Try to modify these rules (bear in mind that the certificates files may
      need to be generated again).
    - Try to generate new rules.

## Troubleshooting
1. Error creating any secure entity
    - Make sure OpenSSL is in the PATH environment variable before running
    LabVIEW. This also applies to the RT Targets, so OpenSSL should be 
	available on the board before deploying the program.
    - Make sure the domain ID for the Reader or Writer you want to create is
    allowed by the permissions file you are loading.
    - Make sure the topic you want to use is allowed by the permissions file 
	you are loading.
    - If you are using an NI Linux RT target, paths to the files to be loaded
    should point to the NI Linux directory structure and the files must be in
    the RT target. You may need to provide a path instead of using 
	`Current VI's path`.
    - If the Administration Panel shows that the length of the properties is
    bigger than 1024 characters, you may need to shorten the path of the 
    security files.
2. Error creating a Custom Secure Profile
    - Make sure all the files that will be used to create the Custom
    Secure Profile exist.
    - Make sure all the files in the basic configuration have been set
    (provided by this profile or inherited from the DomainParticipant base QoS
    Profile).
