<script setup lang="ts">
import Terminal from './Terminal.vue'

// The ClusterAdmissionPolicy shown on the "Blocking non-signed Pods" slide, in the light editor window.
const policy = [
  'apiVersion: policies.kubewarden.io/v1',
  'kind: ClusterAdmissionPolicy',
  'metadata:',
  '  name: check-appco-signatures',
  'spec:',
  '  module: ghcr.io/kubewarden/policies/verify-image-signatures:v0.2.9',
  '  rules:',
  '    - [...]',
  '  namespaceSelector: { ... }',
  '  settings:',
  '    modifyImagesWithDigest: true # mutate the request and use the digest instead of tag',
  '    rule: PublicKey',
  '    signatures:',
  "      - image: '*'",
  '        pubKeys:',
  '          - |-',
  '            -----BEGIN PUBLIC KEY-----',
  '            MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA02FtEt5gBywiyxbmkVsb',
  '            CujcBg5lur0kpEbfDk10gCcs9shVEqEO3ZsOXHursgoaDAWqdPtsYhsgczGeJz9w',
  '            Aw+r6BuRV8YOkE37A8s/7IOQUW0tlqtnt11OKhIiZ9+e5l3ed2H1ymKQO3dgreSy',
  '            rShqYdA3hrItswyp41ApF6zhjSPlR6lAmq3X4wMYLAPptmzfxigTnR4hxB5UNPhs',
  '            i2qA4vLrUM/i+NohECuLr1EAymvupH26HLEdM+eZnlQn+WbhIP5Grc4ba7XrBv7K',
  '            kywgTC7CxkiJZR0mUcUD2wTX/Je8Ewj6oPSalx09e2itzvmU5Kr9XUyME7Zsj5CA',
  '            IwIDAQAB',
  '            -----END PUBLIC KEY-----',
]
</script>

<template>
  <Terminal light yaml title="policy.yaml" icon="{AS}" :lines="policy" style="--term-size: 0.8rem; --term-lh: 1.62; --term-gap: 1.1rem; --term-pad: 0.3rem 1.2rem 0.9rem" />
</template>
