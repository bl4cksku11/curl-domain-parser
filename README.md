parse_curl_domains_tlds.py

# Purpose:
  Parse a curl command (stdin or argument) and extract only domains whose
  TLD (last label) appears in a provided/custom TLD list (to reduce false positives).

# Features:
  - Accepts curl command via STDIN or as a CLI arg
  - Loads TLD set from a local file (--tld-file) or optionally fetches latest IANA list (--fetch-tlds)
  - Normalizes hosts (removes ports/userinfo)
  - Handles --resolve / --connect-to / -H Host: headers / --url etc.
  - Optional output: wildcard patterns (label.*) or a regex for scanners
  - Minimal dependencies (uses stdlib only)

# Usage examples:
  ### 1) read command from stdin and use a local tlds.txt
  echo "curl 'https://login.example.com' -H 'Host: api.exampletechnologies.net'" \
    | python3 parse_curl_domains_tlds.py --tld-file tlds.txt

  ### 2) fetch latest IANA TLDs and save locally, then parse
  python3 parse_curl_domains_tlds.py --fetch-tlds --save-tlds tlds.txt \
    --cmd "curl https://cb.example.co.uk/path -H 'Host: example.exampletechnologies.io'"

  ### 3) produce suggested wildcard patterns
  echo "curl https://login.example.com" \
    | python3 parse_curl_domains_tlds.py --tld-file tlds.txt --wildcards
