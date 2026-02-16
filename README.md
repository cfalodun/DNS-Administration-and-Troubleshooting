# Practicing with DNS

## Objective

Demonstrate DNS record creation, understand client-side DNS caching behavior, and configure CNAME records within an Active Directory environment.

This covers:
- A-record creation
- Name resolution troubleshooting
- DNS cache behavior
- Cache flushing
- CNAME configuration

---

# Part 1 – A Record Creation

## Step 1 – Verify Name Resolution Failure

Logged into:

- DC-1 as mydomain.com\jane_admin
- Client-1 as mydomain.com\jane_admin

From Client-1:

Attempted to ping **mainframe** — failed.

<img src="https://i.postimg.cc/Hk3mPPF4/01-client1-ping-mainframe-fails.png" width="500">

Ran:

nslookup mainframe

No DNS record found.

<img src="https://i.postimg.cc/sgKz00bP/02-client1-nslookup-mainframe-fails.png" width="500">

---

## Step 2 – Create DNS A-Record

On DC-1:

Created new DNS A-record:

Host: mainframe  
IP Address: DC-1 private IP

<img src="https://i.postimg.cc/2STYXXg7/03-dc1-dns-a-record-mainframe.png" width="500">

---

## Step 3 – Verify Successful Resolution

Returned to Client-1.

Pinged **mainframe** again — success.

<img src="https://i.postimg.cc/13MSYYbK/04-client1-ping-mainframe-success.png" width="500">

---

# Part 2 – Local DNS Cache Behavior

## Step 1 – Modify A-Record

Changed mainframe’s IP address on DC-1 to:

8.8.8.8

---

## Step 2 – Observe Cached Result

From Client-1:

Pinged mainframe again.

Observed it still resolves to the old IP address due to DNS caching.

<img src="https://i.postimg.cc/L8vS00GT/05-client1-ping-mainframe-old-ip-cached.png" width="500">

Displayed local DNS cache:

ipconfig /displaydns

<img src="https://i.postimg.cc/nhTxwwN2/06-display-dns-client-1.png" width="500">

---

## Step 3 – Flush DNS Cache

Executed:

ipconfig /flushdns

Verified cache cleared.

Pinged mainframe again — now resolves to updated IP.

<img src="https://i.postimg.cc/P5KHFF0K/07-client1-flush-to-ping-mainframe-new-ip.png" width="500">

---

# Part 3 – CNAME Record Configuration

## Step 1 – Create CNAME Record

On DC-1:

Created CNAME record:

Host: search  
Points to: www.google.com

---

## Step 2 – Verify CNAME Resolution

On Client-1:

Pinged:

search

Ran:

nslookup search

<img src="https://i.postimg.cc/BnN3wwRM/08-client1-cname-search-google.png" width="500">

---

# Skills Demonstrated

- DNS A-record creation
- Forward name resolution troubleshooting
- Understanding DNS caching behavior
- Flushing client-side DNS cache
- CNAME record configuration
- Use of ping and nslookup for validation

---

# Result

Successfully configured and tested:

- A-record resolution
- Record modification impact
- Local cache clearing
- CNAME-based redirection

Demonstrated foundational DNS administration and troubleshooting skills relevant to Help Desk and Junior Systems roles.
