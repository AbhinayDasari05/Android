# DROPGuard — Security Architecture & Cryptographic Principles

## 🔐 Cryptographic Specifications

### 1. PIN Hashing & Password Key Derivation
- **Algorithm**: PBKDF2 with HmacSHA256 (`PBKDF2WithHmacSHA256`)
- **Key Length**: 256 bits
- **Iteration Count**: 10,000 iterations
- **Salt Generation**: 128-bit cryptographically secure random salt generated via `java.security.SecureRandom` per instance.
- **Plaintext Storage**: **NEVER STORED**. Plaintext PINs and passwords are zeroed out after hash generation.

### 2. File Encryption at Rest
- **Algorithm**: AES-256-GCM (`AES/GCM/NoPadding`)
- **Tag Length**: 128 bits (Authentication Tag)
- **Initialization Vector (IV)**: 96-bit unique IV generated per encrypted file and prepended to cipher output payload.
- **Key Storage**: Android KeyStore hardware-backed master key (`DROPGuard_AES_MasterKey`).

### 3. File Integrity Verification
- **Algorithm**: SHA-256 (`MessageDigest.getInstance("SHA-256")`)
- **Process**: Pre-encryption SHA-256 checksum is stored alongside file metadata. Upon decryption, the hash of the decrypted byte array is compared against the stored checksum. If mismatch occurs, decryption throws an `Integrity Verification Failed` exception.

### 4. Storage Security
- **SharedPreferences**: Encrypted using AndroidX `EncryptedSharedPreferences` with `AES256_SIV` key encryption and `AES256_GCM` value encryption.
- **Room Database**: Excludes cloud data extraction rules in `data_extraction_rules.xml`.

---

## 🛡 Authentication & Rate Limiting Policy

To defend against brute-force attacks on the owner's 4-digit PIN:
- **Attempts 1–3**: Normal verification.
- **Attempt 4**: 30-second exponential lockout penalty.
- **Attempt 5**: 60-second lockout penalty.
- **Attempt 6+**: 300-second (5-minute) lockout penalty.

Failed attempts are logged persistently in the Room security audit log database. **User data is NEVER destroyed upon authentication failures to prevent accidental data destruction.**
