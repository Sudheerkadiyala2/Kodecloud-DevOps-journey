# Day 38: Create a Docker Bridge Network
**Date:** 2026-06-16  
**Platform:** KodeKloud DevOps 100 Days Challenge  
**Category:** Docker Networking

## Challenge

The Nautilus DevOps team needed to set up Docker networking for upcoming application deployments.

A ticket was assigned with the following requirements:

- Create a Docker network named **media** on **App Server 1**.
- Configure the network to use the **bridge** driver.
- Set the subnet to **172.168.0.0/24**.
- Set the IP range to **172.168.0.0/24**.

## Solution

### Step 1: SSH into App Server 1

```bash
ssh tony@stapp01
```

### Step 2: Create the Docker Network

```bash
docker network create \
  --driver bridge \
  --subnet 172.168.0.0/24 \
  --ip-range 172.168.0.0/24 \
  media
```

### Step 3: Verify the Network

```bash
docker network inspect media
```

## Commands Used

```bash
ssh tony@stapp01

docker network create \
  --driver bridge \
  --subnet 172.168.0.0/24 \
  --ip-range 172.168.0.0/24 \
  media

docker network inspect media
```

## Verification

Verify that the network was created successfully:

```bash
docker network ls
```

Inspect the network configuration:

```bash
docker network inspect media
```

Expected configuration:

- Network Name: `media`
- Driver: `bridge`
- Subnet: `172.168.0.0/24`
- IP Range: `172.168.0.0/24`

## Outcome

Successfully created the Docker network **media** using the **bridge** driver and configured it with the required subnet and IP range. The network was verified using the Docker inspect command and is ready for container deployments.
