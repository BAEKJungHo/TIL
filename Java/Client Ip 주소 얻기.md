# Client Ip 주소 얻기

```kotlin
// 생략


    override fun doFilterInternal(
        request: HttpServletRequest,
        response: HttpServletResponse,
        filterChain: FilterChain,
    ) {
        val clientIp = getIpXFF(request)

        if (!clientIp.startsWith(WHILTE_LIST_IP)) {
            throw BadCredentialsException("Invalid IP Address: $clientIp")
        }

        filterChain.doFilter(request, response)
    }

    private fun getIpXFF(request: HttpServletRequest): String {
        val ip = request.getHeader("X-Forwarded-For") ?: request.remoteAddr

        if (isMultipleIpXFF(ip)) {
            return getClientIpWhenMultipleIpXFF(ip)
        }

        return ip
    }

    private fun isMultipleIpXFF(ip: String): Boolean {
        return StringUtils.contains(ip, ",")
    }

    private fun getClientIpWhenMultipleIpXFF(ipList: String): String {
        return ipList.split(Regex(pattern = ",")).first()
    }
```

