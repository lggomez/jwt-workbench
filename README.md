# jwt-workbench

This repo contains the basic steps to create valid JWTs mocks with their respective JWKS

#### tl;dr - I just want my JWT/JWKS pair

1. Install https://github.com/leomillon/jwtctl (requires java 1.8)
2. Customize **payloads/claims.json** and **payloads/headers.json** at will. PS. Any change on the `kid` field of the header must be reflected on its JWKS counterpart (unless you actually want to test a JWKS mismatch error ;) )
3. Run the following command: `jwtctl create --claims-file ./payloads/claims.json --rsa-sign RS256 ./PEMs/rsa.pem --headers-file ./payloads/headers.json --verbose`
4. Copy the string output along the the **jwks.json** contents and you're set

Take into account that this will use the preexisting private key pair included in this repo. It is recommended to recreate the entire setup, to do so, please continue to the next section

## JWT how-to

### Key pair and JWKS creation

1. Go to https://mkjwk.org/ and create an RSA key. For this step, it is important to have the following settings:
    * Key Use: Signature
    * Show X.509: Yes
2. Copy the `Public Key` JWKS JSON and `Private Key (X.509 PEM Format)` (for this case, these will be enough, as we'll be signing the token manually with the X.509 key)
3. Convert the X.509 PEM to RSA format with the following command (requires openssl): `openssl rsa -in ./PEMs/x509.pem -out ./PEMs/rsa.pem`
4. Now you can continue to the 1..3 steps of the previous section to create and sign the JWT with this new private key, and using the given JWKS as its valid signing key for mocking/testing scenarios 