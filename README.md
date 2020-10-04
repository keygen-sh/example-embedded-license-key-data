# Example Embedded License Key Data
This is an example of embedding data within cryptographically signed license
keys, and extracting said data out of the keys using your Keygen account's
RSA public key. You can find your public key within
[your account's settings page](https://app.keygen.sh/settings).

Embedding data within cryptographically signed licenses can be used to implement
robuest offline licensing models, as well as adding additional security to your
licensing model. For example, you can embed privileges and expirys within the license's
key which can be extracted offline. All that is needed to cryptographically verify
a license and extract the embedded data is your account's public key.

**For this example, the license's policy _must_ implement the `RSA_2048_PKCS1_PSS_SIGN_V2`
scheme. This example _does not_ cover any other schemes at this time.**

## Running the example

First up, add an environment variable containing your public key:
```bash
# Your Keygen account ID (find yours at https://app.keygen.sh/settings)
export KEYGEN_ACCOUNT_ID="YOUR_KEYGEN_ACCOUNT_ID"

# Keygen product token (don't share this!)
export KEYGEN_PRODUCT_TOKEN="YOUR_KEYGEN_PRODUCT_TOKEN"

# The Keygen policy to use when creating licenses for new users
export KEYGEN_POLICY_ID="YOUR_KEYGEN_POLICY_ID"

# Your Keygen account's public key (make sure it is *exact* - newlines and all)
export KEYGEN_PUBLIC_KEY=$(printf %b \
  '-----BEGIN PUBLIC KEY-----\n' \
  'zdL8BgMFM7p7+FGEGuH1I0KBaMcB/RZZSUu4yTBMu0pJw2EWzr3CrOOiXQI3+6bA\n' \
  # …
  'efK41Ml6OwZB3tchqGmpuAsCEwEAaQ==\n' \
  '-----END PUBLIC KEY-----')
```

You can either run each line above within your terminal session before
starting the app, or you can add the above contents to your `~/.bashrc`
file and then run `source ~/.bashrc` after saving the file.

Next, install dependencies with [`yarn`](https://yarnpkg.comg):
```
yarn
```

## Configuring a license policy

Visit [your dashboard](https://app.keygen.sh/policies) and create a new
policy with the following `scheme` attribute:

```javascript
{
  scheme: 'RSA_2048_PKCS1_PSS_SIGN_V2'
}
```

**This example _must_ use the `RSA_2048_PKCS1_PSS_SIGN_V2` scheme.**

## Generating a license key

To generate a signed license key, run the `generate` script, passing in the some
`data` to embed within the signed key:

```
yarn start generate --data '{"customer":{"name":"ACME Co."},"expiry":1575393876}'
```

## Generating a license key

Then to verify the key, run the `verify` script, passing in the _entire_ signed
`key` you received from the `generate` step above:

```
yarn start verify --key 'key/SOME_SIGNED.LICENSE_KEY_HERE'
```

The license key will be cryptographically verified and if valid, the key's embedded
data will be extracted and displayed. Be sure to copy your public key correctly!
Your keys will fail validation if it is copied incorrectly. You can find your
public key in [your account's settings](https://app.keygen.sh/settings).

## Questions?

Reach out at [support@keygen.sh](mailto:support@keygen.sh) if you have any
questions or concerns!
