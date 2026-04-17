# DNS Administration & Troubleshooting

## Project Summary

This project demonstrates how DNS records are created, modified, and troubleshot in an Active Directory environment.

An A-record was created and tested for name resolution, DNS caching behavior was observed and cleared on the client machine, and a CNAME record was configured to demonstrate alias-based resolution. Tools such as ping, nslookup, and ipconfig were used to validate results.

---

## Environment & Tools

### Environment
- Microsoft Azure Virtual Machines
- Windows Server (Domain Controller – DC-1)
- Windows 10 (Client-1)

### Technologies Used
- Active Directory Integrated DNS
- Windows Networking Tools
---

## Demonstration

# Part 1: A Record Creation

## Step 1: Verify Name Resolution Failure

Log in to:

- **DC-1** as `mydomain.com\jane_admin`
- **Client-1** as `mydomain.com\jane_admin`

From **Client-1**, attempt to resolve a hostname that does not exist.

### Run:
```
ping mainframe
```

The request fails because no DNS record exists.

<img src="https://i.postimg.cc/Hk3mPPF4/01-client1-ping-mainframe-fails.png" width="500">

Next, run:

```
nslookup mainframe
```

This confirms that DNS cannot resolve the name.

<img src="https://i.postimg.cc/sgKz00bP/02-client1-nslookup-mainframe-fails.png" width="500">

---

## Step 2: Create DNS A-Record

On **DC-1**, create a new DNS A-record.

### Steps:
1. Open **DNS Manager**
2. Expand **Forward Lookup Zones**
3. Select your domain (e.g., `mydomain.com`)
4. Right-click → **New Host (A or AAAA)**
5. Enter:
   - Name: `mainframe`
   - IP Address: (DC-1 private IP)
6. Click **Add Host**

<img src="https://i.postimg.cc/2STYXXg7/03-dc1-dns-a-record-mainframe.png" width="500">

---

## Step 3: Verify Successful Resolution

Return to **Client-1** and test again.

```
ping mainframe
```

The hostname now resolves successfully to the configured IP address.

<img src="https://i.postimg.cc/13MSYYbK/04-client1-ping-mainframe-success.png" width="500">

---

# Part 2: Local DNS Cache Behavior

## Step 1: Modify A-Record

On **DC-1**, modify the existing A-record:

- Change the IP address of **mainframe** to:
```
8.8.8.8
```

---

## Step 2: Observe Cached Result

From **Client-1**, run:

```
ping mainframe
```

It may still resolve to the old IP address due to DNS caching.

<img src="https://i.postimg.cc/L8vS00GT/05-client1-ping-mainframe-old-ip-cached.png" width="500">

To confirm cached entries, run:

```
ipconfig /displaydns
```

<img src="https://i.postimg.cc/nhTxwwN2/06-display-dns-client-1.png" width="500">

---

## Step 3: Flush DNS Cache

Clear the local DNS cache:

```
ipconfig /flushdns
```

Then test again:

```
ping mainframe
```

The hostname now resolves to the updated IP address.

<img src="https://i.postimg.cc/P5KHFF0K/07-client1-flush-to-ping-mainframe-new-ip.png" width="500">

---

# Part 3: CNAME Record Configuration

## Step 1: Create CNAME Record

On **DC-1**, create a CNAME record.

### Steps:
1. Open **DNS Manager**
2. Navigate to your domain zone
3. Right-click → **New Alias (CNAME)**
4. Enter:
   - Alias Name: `search`
   - Fully Qualified Domain Name (FQDN): `www.google.com`
5. Click **OK**

---

## Step 2: Verify CNAME Resolution

On **Client-1**, run:

```
ping search
```

Then:

```
nslookup search
```

<img src="https://i.postimg.cc/BnN3wwRM/08-client1-cname-search-google.png" width="500">

This confirms that the alias resolves to the target domain.
