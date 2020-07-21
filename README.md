## 20200721在ubuntu云上的部署
### 安装docker

'''
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
'''

### Pull the image

```
docker pull seedgou/zerotier-moon
```

### 云安全组放开udp:9994

### run
'''
docker run --name zerotier-moon -d -p 9994:9993/udp -v ~/zerotier-moon-config:/var/lib/zerotier-one seedgou/zerotier-moon -4 X.X.X.X -p 9994
'''

### 查看结果
'''
docker logs zerotier-moon
'''

### 客户端orbit大概的方式
zerotier-cli orbit 1234567890 1234567890


# docker-zerotier-moon
A docker image to create ZeroTier moon in one setp.

## Usage

### Pull the image

```
docker pull seedgou/zerotier-moon
```

### Start a container

```
docker run --name zerotier-moon -d --restart always -p 9993:9993/udp seedgou/zerotier-moon -4 1.2.3.4
```
 
Replace `1.2.3.4` with your moon's IP.

To show your moon id, run

```
docker logs zerotier-moon
```

**Notice:**
When creating a new container, a new moon id will be generated. To persist the identity when creating a new container, see **Mount ZeroTier conf folder** below.

## Advanced usage

### Manage ZeroTier

```
docker exec zerotier-moon /zerotier-cli
```

### Mount ZeroTier conf folder

```
docker run --name zerotier-moon -d -p 9993:9993/udp -v ~/somewhere:/var/lib/zerotier-one seedgou/zerotier-moon -4 1.2.3.4 -6 2001:abcd:abcd::1
```

This will mount `~/somewhere` to `/var/lib/zerotier-one` inside the container, allowing your ZeroTier moon to presist the same moon id.  If you don't do this, when you start a new container, a new moon id will be generated.

### IPv6 support

```
docker run --name zerotier-moon -d -p 9993:9993/udp seedgou/zerotier-moon -4 1.2.3.4 -6 2001:abcd:abcd::1
```

Replace `1.2.3.4`, `2001:abcd:abcd::1` with your moon's IP. You can remove `-4` option in pure IPv6 environment.

### Custom port

```
docker run --name zerotier-moon -d -p 9994:9993/udp seedgou/zerotier-moon -4 1.2.3.4 -p 9994
```

Replace 9994 with your own custom port for ZeroTier moon.
