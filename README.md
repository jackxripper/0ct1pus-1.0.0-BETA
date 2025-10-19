## Description

A bash-based Instagram brute force tool that attempts password attacks using wordlists through TOR network routing.

## Author

Work done by jackxripper

## Requirements

- Root access
- TOR service running
- Required dependencies:
  - openssl
  - tor
  - curl
  - awk
  - sed
  - cat
  - tr
  - wc
  - cut
  - uniq
  - /dev/urandom

## Features

- Multi-threaded password attempts (configurable threads, recommended < 20)
- TOR network integration for anonymized requests
- Session save/resume functionality
- Automatic IP rotation via TOR circuit changes
- Device ID and GUID spoofing
- Instagram API authentication emulation
- Password wordlist support

## Installation

1. Ensure all dependencies are installed
2. Start TOR service:
   ```bash
   tor
   # or
   service tor start
   ```
3. Make the script executable:
   ```bash
   chmod +x instashell.sh
   ```

## Usage

### Basic Attack
```bash
sudo ./instashell.sh
```

You will be prompted for:
- Target Instagram username
- Password list file (default: `passwords.lst`)
- Number of threads (default: 10)

### Resume Session
```bash
sudo ./instashell.sh --resume
```

## How It Works

1. **Initialization**: Generates random device identifiers (UUID, GUID, phone ID, device ID)
2. **Account Validation**: Checks if the target username exists
3. **TOR Connection**: Verifies TOR is running and accessible
4. **Brute Force Loop**:
   - Reads passwords from wordlist in batches
   - Creates authenticated requests with HMAC-SHA256 signatures
   - Routes requests through TOR (127.0.0.1:9050)
   - Monitors for success responses (200, challenge)
   - Rotates IP on rate limiting
5. **Result Storage**: Saves found passwords to `found.passwords`

## Session Management

- Press `Ctrl+C` to stop and optionally save session
- Sessions stored in `sessions/` directory
- Resume saved sessions with `--resume` flag

## Output

- Real-time password attempt display
- Success indicators for found passwords
- Results saved to `found.passwords` file

## Technical Details

- Uses Instagram API endpoint: `https://i.instagram.com/api/v1/accounts/login/`
- Spoofs Android device (Instagram 10.26.0 Android)
- Implements HMAC-SHA256 request signing
- SOCKS5 proxy through TOR (localhost:9050)
- Automatic IP rotation on rate limit detection

## Signals

- `SIGINT` (Ctrl+C): Triggers session save prompt
- `SIGHUP`: Reloads TOR for new circuit

## Notes

- Requires root privileges to run
- Default thread count is 10 (keep under 20 for stability)
- Automatic IP rotation helps avoid rate limiting
- Sessions include user, last attempted password, and wordlist path
