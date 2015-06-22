How to Run the Servers

  java -jar issuer/selfcontained-issuance-service.war 9100

  java -jar user/selfcontained-user-service.war 9200

  java -jar verifier/selfcontained-verification-service.war 9300

  java -jar inspector/selfcontained-inspection-service.war 9400

  java -jar revocation/selfcontained-revocation-service.war 9500
  
  java -jar identity/selfcontained-identity-service.war 9600
  
  --- Setup System Parameters ---


 #!/bin/sh

 #Stop script if an error occurs.
 set -e
 # Setup System Parameters.
 echo "Setup System Parameters"
 curl -X POST --header 'Content-Type: text/xml' 'http://localhost:9100/issuer/setupSystemParameters/?keyLength=1024' > new/systemparameters.xml

 # Store credential specification at issuer.
 echo "Store credential specification at issuer"
 curl -X PUT --header 'Content-Type: text/xml' -d @tutorial-resources/credentialSpecificationMybank.xml 'http://localhost:9100/issuer/storeCredentialSpecification/http%3A%2F%2Fmybank%2Fcredential' > new/storeCredentialSpecificationAtIssuerResponce.xml


 # Store credential specification at user.
 # This method is not specified in H2.2.
 echo "Store credential specification at user"
 curl -X PUT --header 'Content-Type: text/xml' -d @tutorial-resources/credentialSpecificationMybank.xml 'http://localhost:9200/user/storeCredentialSpecification/http%3A%2F%2Fmybank%2Fcredential' > new/storeCredentialSpecificationAtUserResponce.xml

 # Store credential specification at verifier.
 # This method is not specified in H2.2.
 echo "Store credential specification at verifier"
 curl -X PUT --header 'Content-Type: text/xml' -d @tutorial-resources/credentialSpecificationMybank.xml  'http://localhost:9300/verification/storeCredentialSpecification/http%3A%2F%2Fmybank%2Fcredential' > new/storeCredentialSpecificationAtVerifierResponce.xml

 # Store System parameters at Revocation Authority.
 # This method is not specified in H2.2.
 echo "Store System parameters at Revocation Authority"
 curl -X POST --header 'Content-Type: text/xml' -d @new/systemparameters.xml 'http://localhost:9500/revocation/storeSystemParameters/' > new/storeSystemParametersResponceAtRevocationAutority.xml

 # Store System parameters at User.
 # This method is not specified in H2.2.
 echo "Store System parameters at User"
 curl -X POST --header 'Content-Type: text/xml' -d @new/systemparameters.xml 'http://localhost:9200/user/storeSystemParameters/' > new/storeSystemParametersResponceAtUser.xml

 # Store System parameters at verifier.
 # This method is not specified in H2.2.
 echo "Store System parameters at Verifier"
 curl -X POST --header 'Content-Type: text/xml' -d @new/systemparameters.xml 'http://localhost:9300/verification/storeSystemParameters/' > new/storeSystemParametersResponceAtVerifier.xml

 # Setup Revocation Authority Parameters.
 echo "Setup Revocation Authority Parameters"
 curl -X POST --header 'Content-Type: text/xml' -d @tutorial-resources/revocationReferences.xml 'http://localhost:9500/revocation/setupRevocationAuthorityParameters?keyLength=1024&uid=http%3A%2F%2Fmybank%2Frevocation' > new/revocationAuthorityParameters.xml

 # Store Revocation Authority Parameters at issuer.
 # This method is not specified in H2.2.
 echo "Store Revocation Authority Parameters at issuer"
 curl -X PUT --header 'Content-Type: text/xml' -d @new/revocationAuthorityParameters.xml 'http://localhost:9100/issuer/storeRevocationAuthorityParameters/http%3A%2F%2Fmybank%2Frevocation'  > new/storeRevocationAuthorityParameters.xml

 # Store Revocation Authority Parameters at user.
 # This method is not specified in H2.2.
 echo "Store Revocation Authority Parameters at user"
 curl -X PUT --header 'Content-Type: text/xml' -d @new/revocationAuthorityParameters.xml 'http://localhost:9200/user/storeRevocationAuthorityParameters/http%3A%2F%2Fmybank%2Frevocation'  > new/storeRevocationAuthorityParametersAtUserResponce.xml

 # Store Revocation Authority Parameters at verifier.
 # This method is not specified in H2.2.
 echo "Store Revocation Authority Parameters at verifier"
 curl -X PUT --header 'Content-Type: text/xml' -d @new/revocationAuthorityParameters.xml 'http://localhost:9300/verification/storeRevocationAuthorityParameters/http%3A%2F%2Fmybank%2Frevocation'  > new/storeRevocationAuthorityParametersAtVerifierResponce.xml

 ##

 # Store System parameters at Inspector.
 # This method is not specified in H2.2.
 echo "Store System parameters at inspector"
 curl -X POST --header 'Content-Type: text/xml' -d @new/systemparameters.xml 'http://localhost:9400/inspector/storeSystemParameters/' > new/storeSystemParametersResponceAtInspector.xml

 # Store credential specification at Inspector.
 # This method is not specified in H2.2.
 echo "Store credential specification at inspector"
 curl -X PUT --header 'Content-Type: text/xml' -d @tutorial-resources/credentialSpecificationMybank.xml 'http://localhost:9400/inspector/storeCredentialSpecification/http%3A%2F%2Fmybank%2Fcredential' > new/storeCredentialSpecificationAtInspectorResponce.xml


 # Generate Inspector Public Key
 # This method is not specified in H2.2.
 echo "Generating Inspector Public Key"
 curl -X POST --header 'Content-Type: text/xml' 'http://localhost:9400/inspector/setupInspectorPublicKey?keyLength=1024&cryptoMechanism=idemix&uid=http%3A%2F%2Fmybank%2Finspection' > new/inspectorPublicKey.xml

 # Store Inspector Public Key at user.
 # This method is not specified in H2.2.
 echo "Store Inspector Public Key at user"
 curl -X PUT --header 'Content-Type: text/xml' -d @new/inspectorPublicKey.xml 'http://localhost:9200/user/storeInspectorPublicKey/http%3A%2F%2Fticketcompany%2Finspection'  > new/storeInspectorPublicKeyAtUserResponce.xml

 # Store Inspector Public Key at verifier.
 # This method is not specified in H2.2.
 echo "Store Inspector Public Key at verifier"
 curl -X PUT --header 'Content-Type: text/xml' -d @new/inspectorPublicKey.xml 'http://localhost:9300/verification/storeInspectorPublicKey/http%3A%2F%2Fticketcompany%2Finspection'  > new/storeInspectorPublicKeyAtVerifierResponce.xml


 ##

 # Setup issuer parameters.
 echo "Setup issuer parameters"
 curl -X POST --header 'Content-Type: text/xml' -d @tutorial-resources/issuerParametersInput.xml 'http://localhost:9100/issuer/setupIssuerParameters/' > new/issuerParameters.xml


 # Store Issuer Parameters at user.
 # This method is not specified in H2.2.
 echo "Store Issuer Parameters at user"
 curl -X PUT --header 'Content-Type: text/xml' -d @new/issuerParameters.xml 'http://localhost:9200/user/storeIssuerParameters/http%3A%2F%2Fmybank%2Fissuance%3Aidemix' > new/storeIssuerParametersAtUser.xml

 # Store Issuer Parameters at verifier.
 # This method is not specified in H2.2.
 echo "Store Issuer Parameters at verifier"
 curl -X PUT --header 'Content-Type: text/xml' -d @new/issuerParameters.xml 'http://localhost:9300/verification/storeIssuerParameters/http%3A%2F%2Fmybank%2Fissuance%3Aidemix' > new/storeIssuerParametersAtVerifier.xml

 # Create smartcard at user.
 # This method is not specified in H2.2.
 echo "Create smartcard at user"
 curl -X POST --header 'Content-Type: text/xml' 'http://localhost:9200/user/createSmartcard/http%3A%2F%2Fmybank%2Fissuance%3Aidemix'

 # Init issuance protocol (first step for the issuer).
 echo "Init issuance protocol"
 curl -X POST --header 'Content-Type: text/xml' -d @tutorial-resources/issuancePolicyAndAttributes.xml 'http://localhost:9100/issuer/initIssuanceProtocol/' > new/issuanceMessageAndBoolean.xml

 # Extract issuance message.
 curl -X POST --header 'Content-Type: text/xml' -d @new/issuanceMessageAndBoolean.xml 'http://localhost:9200/user/extractIssuanceMessage/' > new/firstIssuanceMessage.xml

 # First issuance protocol step (first step for the user).
 echo "First issuance protocol step for the user"
 curl -X POST --header 'Content-Type: text/xml' -d @new/firstIssuanceMessage.xml 'http://localhost:9200/user/issuanceProtocolStep/' > new/issuanceReturn.xml

 echo "Select first usable identity"
 curl -X POST --header 'Content-Type: text/xml' -d @new/issuanceReturn.xml 'http://localhost:9600/identity/issuance/' > new/uiIssuanceReturn.xml

 # First issuance protocol step - UI (first step for the user).
 echo "Second issuance protocol step (first step for the user)"
 curl -X POST --header 'Content-Type: text/xml' -d @new/uiIssuanceReturn.xml 'http://localhost:9200/user/issuanceProtocolStepUi/' > new/secondIssuanceMessage.xml

 # Second issuance protocol step (second step for the issuer).
 echo "Second issuance protocol step (second step for the issuer)"
 curl -X POST --header 'Content-Type: text/xml' -d @new/secondIssuanceMessage.xml 'http://localhost:9100/issuer/issuanceProtocolStep/' > new/thirdIssuanceMessageAndBoolean.xml

 # Extract issuance message.
 curl -X POST --header 'Content-Type: text/xml' -d @new/thirdIssuanceMessageAndBoolean.xml 'http://localhost:9200/user/extractIssuanceMessage/' > new/thirdIssuanceMessage.xml

 # Third issuance protocol step (second step for the user).
 echo "Third issuance protocol step (second step for the user)"
 curl -X POST --header 'Content-Type: text/xml' -d @new/thirdIssuanceMessage.xml 'http://localhost:9200/user/issuanceProtocolStep/' > new/fourthIssuanceMessageAndBoolean.xml

 # Create presentation policy alternatives.
 # This method is not specified in H2.2.
 echo "Create presentation policy alternatives"
 curl -X GET --header 'Content-Type: text/xml' -d @tutorial-resources/presentationPolicyAlternatives.xml 'http://localhost:9300/verification/createPresentationPolicy?applicationData=testData' > new/presentationPolicyAlternatives.xml

 # Create presentation UI return.
 # This method is not specified in H2.2.
 echo "Create presentation UI return"
 curl -X POST --header 'Content-Type: text/xml' -d @new/presentationPolicyAlternatives.xml 'http://localhost:9200/user/createPresentationToken/' > new/uiPresentationArguments.xml

 echo "Select first posisble identity"
 curl -X POST --header 'Content-Type: text/xml' -d @new/uiPresentationArguments.xml 'http://localhost:9600/identity/presentation' > new/uiPresentationReturn.xml
  
 # Create presentation token.
 # This method is not specified in H2.2.
 echo "Create presentation token"
 curl -X POST --header 'Content-Type: text/xml' -d @new/uiPresentationReturn.xml 'http://localhost:9200/user/createPresentationTokenUi/' > new/presentationToken.xml

 # Setup presentationPolicyAlternativesAndPresentationToken.xml.
 #This part is broken. the <?xml version...> of the ppa and pt needs to be stripped
 presentationPolicy=`cat new/presentationPolicyAlternatives.xml | sed 's/^.*<abc:PresentationPolicyAlternatives xmlns:xs="http:\/\/www.w3.org\/2001\/XMLSchema" xmlns:abc="http:\/\/abc4trust.eu\/wp2\/abcschemav1.0" xmlns:idmx="http:\/\/zurich.ibm.com" xmlns:xsi="http:\/\/www.w3.org\/2001\/XMLSchema-instance" Version="1.0">//'`

 presentationToken=`cat new/presentationToken.xml | sed 's/^.*<abc:PresentationToken xmlns:xs="http:\/\/www.w3.org\/2001\/XMLSchema" xmlns:abc="http:\/\/abc4trust.eu\/wp2\/abcschemav1.0" xmlns:idmx="http:\/\/zurich.ibm.com" xmlns:xsi="http:\/\/www.w3.org\/2001\/XMLSchema-instance" Version="3.0.0">//'`
 # echo "${presentationPolicy}"
 # echo "${presentationToken}"
 echo '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>' > new/presentationPolicyAlternativesAndPresentationToken.xml
 echo '<abc:PresentationPolicyAlternativesAndPresentationToken xmlns:abc="http://abc4trust.eu/wp2/abcschemav1.0" Version="1.0"> <abc:PresentationPolicyAlternatives>' >> new/presentationPolicyAlternativesAndPresentationToken.xml
 echo "${presentationPolicy}" >> new/presentationPolicyAlternativesAndPresentationToken.xml
 #echo '</PresentationPolicyAlternatives>' >> presentationPolicyAlternativesAndPresentationToken.xml
 echo '<abc:PresentationToken>' >> new/presentationPolicyAlternativesAndPresentationToken.xml
 echo "${presentationToken}" >> new/presentationPolicyAlternativesAndPresentationToken.xml
 #echo '</PresentationToken>' >> presentationPolicyAlternativesAndPresentationToken.xml
 echo '</abc:PresentationPolicyAlternativesAndPresentationToken>' >> new/presentationPolicyAlternativesAndPresentationToken.xml
  
 # Verify presentation token against presentation policy.
 echo "Verify presentation token against presentation policy"
 # This method is not specified in H2.2.
 curl -X POST --header 'Content-Type: text/xml' -d @new/presentationPolicyAlternativesAndPresentationToken.xml 'http://localhost:9300/verification/verifyTokenAgainstPolicy/' > new/presentationTokenDescription.xml

 #
 # Inspect presentation token.
 echo "Inspect presentation token"
 curl -X POST --header 'Content-Type: text/xml' -d @new/presentationToken.xml 'http://localhost:9400/inspector/inspect/' > new/inspectResult.xml


 #
 # Find revocation handle and revoke the credential.
 echo "Constructing revocation request"
 revocationhandle=`cat new/fourthIssuanceMessageAndBoolean.xml | sed 's/^.*revocationhandle" DataType="xs:integer" Encoding="urn:abc4trust:1.0:encoding:integer:unsigned"\/><abc:AttributeValue xsi:type="xs:integer">//' | sed 's/<.*//'`
 echo "${revocationhandle}"

 echo '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
 <abc:AttributeList xmlns:xs="http://www.w3.org/2001/XMLSchema"
     xmlns:abc="http://abc4trust.eu/wp2/abcschemav1.0" 
     xmlns:idmx="http://zurich.ibm.com"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
     <abc:Attributes>
       <abc:AttributeUID>urn:abc4trust:1.0:attribute/NOT_USED</abc:AttributeUID>
       <abc:AttributeDescription Type="http://abc4trust.eu/wp2/abcschemav1.0/revocationhandle"
            DataType="xs:integer" 
            Encoding="urn:abc4trust:1.0:encoding:integer:unsigned"/>
       <abc:AttributeValue xsi:type="xs:integer">' >> new/revocationAttributeList.xml
 echo "${revocationhandle}" >> new/revocationAttributeList.xml
 echo '</abc:AttributeValue></abc:Attributes></abc:AttributeList>' >> new/revocationAttributeList.xml

 echo "Calling revocation authority"
 curl -X POST --header 'Content-Type: text/xml' -d @new/revocationAttributeList.xml 'http://localhost:9500/revocation/revoke/http%3A%2F%2Fmybank%2Frevocation' > new/revokeReply.xml

 #
 # Check if the previous policy can be satisfied after revocation. Should return false.

 echo "Checking if policy can be satisfied."
 curl -X POST --header 'Content-Type: text/xml' -d @new/presentationPolicyAlternatives.xml 'http://localhost:9200/user/canBeSatisfied/' > new/canBeSatisfied.xml





 