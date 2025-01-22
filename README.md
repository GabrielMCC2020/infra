# infra
Inventarios de Infraestructura

OPCION 1:
Write-Host "Marca:" -ForegroundColor Green $(Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object -ExpandProperty Manufacturer); Write-Host "Modelo:" -ForegroundColor Green $(Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object -ExpandProperty Model); Write-Host "Número de Serie:" -ForegroundColor Green $(Get-CimInstance -ClassName Win32_BIOS | Select-Object -ExpandProperty SerialNumber); Write-Host "Hostname:" -ForegroundColor Green $env:COMPUTERNAME; Write-Host "Sistema Operativo:" -ForegroundColor Green "$(Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object -ExpandProperty Caption) $(Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object -ExpandProperty Version)"; Write-Host "Dirección IP:" -ForegroundColor Green $((Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -notlike '*Virtual*' -and $_.IPAddress -notmatch '169.254.*' -and $_.PrefixOrigin -eq 'Dhcp' }).IPAddress)

OPCION 2:
Write-Host "Tipo de dispositivo: $(If (Get-CimInstance -ClassName Win32_Battery) { 'Laptop' } Else { 'Computadora de escritorio' })"; Write-Host "Marca: $(Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object -ExpandProperty Manufacturer)"; Write-Host "Modelo: $(Get-CimInstance -ClassName Win32_ComputerSystem | Select-Object -ExpandProperty Model)"; Write-Host "Número de Serie: $(Get-CimInstance -ClassName Win32_BIOS | Select-Object -ExpandProperty SerialNumber)"; Write-Host "Hostname: $env:COMPUTERNAME"; Write-Host "Sistema Operativo: $(Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object -ExpandProperty Caption) $(Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object -ExpandProperty Version)"; Write-Host "Procesador: $(Get-CimInstance -ClassName Win32_Processor | Select-Object -ExpandProperty Name)"; Write-Host "Conexión activa: $(If ((Get-NetAdapter | Where-Object { $_.Status -eq 'Up' }).InterfaceDescription -match 'Wireless') { 'WiFi' } Else { 'Cable (Ethernet)' })"; $ipInfo = Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -notlike '*Virtual*' -and $_.IPAddress -notmatch '169.254.*' -and $_.PrefixOrigin -eq 'Dhcp' }; Write-Host "Dirección IPv4: $($ipInfo.IPAddress)"; $subnetMask = (Get-NetIPAddress -IPAddress $ipInfo.IPAddress).PrefixLength; Write-Host "Máscara de subred: $(ConvertTo-SubnetMask $subnetMask)"; Write-Host "Puerta de enlace predeterminada: $(Get-NetRoute -DestinationPrefix '0.0.0.0/0' | Where-Object { $_.InterfaceAlias -eq $ipInfo.InterfaceAlias } | Select-Object -ExpandProperty NextHop)"; Write-Host "Dirección MAC: $((Get-NetAdapter | Where-Object { $_.Status -eq 'Up' }).MacAddress)"; Write-Host "Servidores DNS: $((Get-DnsClientServerAddress -AddressFamily IPv4 | Select-Object -ExpandProperty ServerAddresses | Sort-Object -Unique))"; Write-Host "Última actualización de Windows: $(Get-CimInstance -ClassName Win32_OperatingSystem | Select-Object -ExpandProperty LastBootUpTime)"; Write-Host "Nombre de usuario: $env:USERNAME"

