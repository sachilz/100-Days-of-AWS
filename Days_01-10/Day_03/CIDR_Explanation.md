# AWS VPC `/16` and Subnet `/20` — CIDR Explained

## 1. The basic idea

In this AWS example:

```text
VPC     = 172.31.0.0/16
Subnets = /20
```

The VPC is the **larger network**, and the subnets are **smaller networks inside the VPC**.

Think of it like this:

```text
VPC /16
└── Subnet /20
    └── EC2 / other resources
```

---

## 2. What does `/16` mean?

The VPC is:

```text
172.31.0.0/16
```

IPv4 addresses contain **32 bits**.

`/16` means:

```text
Network portion = 16 bits
Host portion    = 16 bits
```

Therefore, the VPC contains:

```text
2^16 = 65,536 total IP addresses
```

The range is:

```text
172.31.0.0 → 172.31.255.255
```

So:

```text
┌───────────────────────────────────────────┐
│              VPC: /16                     │
│                                           │
│  172.31.0.0 → 172.31.255.255             │
│                                           │
└───────────────────────────────────────────┘
```

---

## 3. What does `/20` mean?

A subnet can be:

```text
172.31.0.0/20
```

`/20` means:

```text
Network portion = 20 bits
Host portion    = 12 bits
```

Therefore:

```text
2^12 = 4,096 total IP addresses
```

For example:

```text
172.31.0.0/20
```

has the range:

```text
172.31.0.0 → 172.31.15.255
```

So a `/20` subnet is smaller than the `/16` VPC.

---

## 4. How can a `/16` VPC contain `/20` subnets?

The `/16` VPC is the big network.

It can be divided into smaller `/20` networks.

```text
VPC: 172.31.0.0/16

┌──────────────┬──────────────┬──────────────┐
│ /20          │ /20          │ /20          │
│ 0 - 15       │ 16 - 31      │ 32 - 47      │
├──────────────┼──────────────┼──────────────┤
│ /20          │ /20          │ /20          │
│ 48 - 63      │ 64 - 79      │ 80 - 95      │
├──────────────┼──────────────┼──────────────┤
│ /20          │ /20          │ /20          │
│ 96 - 111     │ 112 - 127    │ 128 - 143    │
├──────────────┼──────────────┼──────────────┤
│ ...          │ ...          │ ...          │
└──────────────┴──────────────┴──────────────┘
```

The VPC is the **container**, while each subnet is a **smaller network inside it**.

---

## 5. Why does the third octet increase by 16?

This is one of the most important parts.

A `/20` subnet has this subnet mask:

```text
255.255.240.0
```

Look at the third octet:

```text
240
```

The block size is:

```text
256 - 240 = 16
```

Therefore, `/20` subnet boundaries occur every 16 in the third octet:

```text
0
16
32
48
64
80
96
112
128
144
160
176
192
208
224
240
```

That gives the possible `/20` networks inside the `/16` VPC.

---

## 6. The subnets in the AWS example

The existing subnets are:

| Subnet | IP Range |
|---|---|
| `172.31.0.0/20` | `172.31.0.0 – 172.31.15.255` |
| `172.31.16.0/20` | `172.31.16.0 – 172.31.31.255` |
| `172.31.32.0/20` | `172.31.32.0 – 172.31.47.255` |
| `172.31.48.0/20` | `172.31.48.0 – 172.31.63.255` |
| `172.31.64.0/20` | `172.31.64.0 – 172.31.79.255` |
| `172.31.80.0/20` | `172.31.80.0 – 172.31.95.255` |

The next available non-overlapping `/20` is:

```text
172.31.96.0/20
```

Its range is:

```text
172.31.96.0 → 172.31.111.255
```

Therefore, for the KodeKloud task:

```text
Subnet name:
datacenter-subnet

CIDR:
172.31.96.0/20
```

---

## 7. How many `/20` subnets fit inside `/16`?

Use this formula:

```text
Subnet prefix - VPC prefix
```

Here:

```text
20 - 16 = 4
```

Then:

```text
2^4 = 16
```

Therefore:

> A `/16` network can be divided into **16 `/20` networks**.

They are:

```text
1.  172.31.0.0/20
2.  172.31.16.0/20
3.  172.31.32.0/20
4.  172.31.48.0/20
5.  172.31.64.0/20
6.  172.31.80.0/20
7.  172.31.96.0/20
8.  172.31.112.0/20
9.  172.31.128.0/20
10. 172.31.144.0/20
11. 172.31.160.0/20
12. 172.31.176.0/20
13. 172.31.192.0/20
14. 172.31.208.0/20
15. 172.31.224.0/20
16. 172.31.240.0/20
```

---

## 8. Visualizing the whole thing

```text
                    VPC
              172.31.0.0/16
                     │
        ┌────────────┴────────────┐
        │                         │
        ▼                         ▼
   16 smaller networks       /20 subnets
        │
        ├── 172.31.0.0/20
        ├── 172.31.16.0/20
        ├── 172.31.32.0/20
        ├── 172.31.48.0/20
        ├── 172.31.64.0/20
        ├── 172.31.80.0/20
        ├── 172.31.96.0/20
        ├── ...
        └── 172.31.240.0/20
```

---

## 9. Important CIDR rule

Remember:

> **The smaller the prefix number, the larger the network.**

For example:

```text
/16  → larger network
/20  → smaller network
/24  → even smaller network
```

So:

```text
/16 > /20 > /24
```

in terms of **network size**.

---

## 10. AWS-specific detail

A `/20` subnet contains:

```text
2^12 = 4,096 total IPv4 addresses
```

However, AWS reserves **5 IP addresses in every subnet**.

Therefore:

```text
4,096 - 5 = 4,091
```

So a `/20` subnet provides **4,091 usable IPv4 addresses for AWS resources**.

---

## 11. Final answer for the KodeKloud task

The task asks you to create one subnet under the **default VPC**.

Use:

```text
VPC:
Default VPC

Subnet name:
datacenter-subnet

IPv4 CIDR:
172.31.96.0/20
```

The reason `172.31.96.0/20` works is that the existing subnets occupy:

```text
172.31.0.0/20
172.31.16.0/20
172.31.32.0/20
172.31.48.0/20
172.31.64.0/20
172.31.80.0/20
```

and `172.31.96.0/20` is the next non-overlapping `/20` block.

---

## Quick memory trick

```text
VPC = BIG network
Subnet = SMALL network inside VPC

/16 → 65,536 addresses
/20 → 4,096 addresses

/16 → can contain 16 × /20 networks

/20 subnet boundaries:
0, 16, 32, 48, 64, 80, 96, 112...
```

### One-line summary

**A `/16` VPC is a large IP address space, and `/20` subnets divide that space into smaller, non-overlapping networks where AWS resources such as EC2 instances can be placed.**
