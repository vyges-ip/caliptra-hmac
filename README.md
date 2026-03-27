# Caliptra HMAC

HMAC-384/512 authentication engine from the [CHIPS Alliance Caliptra Root of Trust](https://github.com/chipsalliance/caliptra-rtl).

## Features

- **Algorithms**: HMAC-384 and HMAC-512 (based on masked SHA-512)
- **Key size**: 512 bits (16 x 32-bit words)
- **Block size**: 1024 bits (32 x 32-bit words)
- **Tag size**: 384 or 512 bits
- **Bus**: AHB-Lite slave (32-bit)
- **KeyVault**: 2 read ports (key + block) and 1 write port (tag)
- **Entropy**: 12 LFSR generators (384-bit entropy) for masked SHA-512
- **CSR key**: Direct key input from Caliptra CSR (cptra_csr_hmac_key)
- **Security**: Zeroize, masked SHA-512 core, debug unlock protection

## Architecture

```
hmac_ctrl (AHB-Lite slave wrapper)
├── ahb_slv_sif (AHB adapter)
└── hmac (wrapper)
    ├── hmac_core (HMAC algorithm: IPAD/OPAD/SHA state machine)
    │   └── sha512_masked_core x2 (masked SHA-512 instances)
    ├── hmac_lfsr x12 (entropy generators)
    ├── hmac_reg (register file, PeakRDL generated)
    ├── kv_read_client x2 (KeyVault key + block read)
    └── kv_write_client (KeyVault tag write)
```

## Upstream

Extracted from [chipsalliance/caliptra-rtl](https://github.com/chipsalliance/caliptra-rtl) `src/hmac`. See `upstream.yaml` for commit tracking.

## License

Apache-2.0 — see [LICENSE](LICENSE).
