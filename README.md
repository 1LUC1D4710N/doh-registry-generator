


# DoH Registry Generator

## How to Run This Tool

**Local usage:**
1. Download or clone this repository.
2. Open `doh-registry-generator.html` in any modern browser (Edge, Chrome, Firefox, etc.).
3. Use the interactive page to select DNS providers and generate your registry file.

**Online usage (GitHub Pages):**
1. Go to the repository’s Settings > Pages.
2. Set the source to the `main` branch and `/root` (or `/docs` if you move the file).
3. Save and use the public URL provided by GitHub Pages to access the tool online.

No extra setup or installation is required—just open the file and use!

Generate custom Windows registry files for DNS-over-HTTPS (DoH) Well Known Servers with a simple, interactive HTML tool. Select only the DNS providers you want—your registry file is fully customized to your choices.

## Features

- Select only the DNS providers you want—full customization
- Instantly generate registry entries for your choices
- Download the ready-to-import `.reg` file
- Modern, user-friendly interface

## How to Use

1. Open `doh-registry-generator.html` in your browser
2. Checkmark only the DNS providers you want
3. Click "Generate Registry" to preview your custom file
4. Download the `.reg` file
5. Import the `.reg` file into Windows (double-click or use Registry Editor)
6. Reboot your system
7. Set your desired DNS provider in Windows network settings

## Inspiration & Further Info

Check the HTML source of this page for inspiration on how it’s made. This project was inspired by my prior Extended DoH Well Known Servers release and the idea that everyone should be able to choose their own DNS providers easily.


---



## DoH Settings Guide (DohWellKnownServers)

This tool and most of this README focus on DohWellKnownServers, which sets system-wide DoH templates for Windows. You can manually add your own IPv4 or IPv6 DoH entries to the registry:

```registry
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters\DohWellKnownServers\IPV4-or-IPv6]
"Template"="DoH-ADDRESS"
```

Replace `IPV4-or-IPv6` with your DNS server IP and `DoH-ADDRESS` with the DoH template URL.

---

**Note:** The section below is only for configuring per-adapter (interface-specific) DoH using InterfaceSpecificParameters. Do not mix these two registry areas.

---


## Powerusers for Interface Specific Parameters

This section is for power users and corporate setups who want to configure DNS-over-HTTPS (DoH) for specific network adapters using InterfaceSpecificParameters. This is not for the main DohWellKnownServers autotemplate.

Find your adapter GUIDs with PowerShell:

```powershell
Get-NetAdapter | Select-Object Name, InterfaceGuid
```


Registry paths for per-adapter for Interface specific parameters:

- **IPv4 should be recognized as Doh:**

	```registry
	[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\InterfaceSpecificParameters\{Interface-GUID}\DohProfileSettings\Doh]
	```

	Then entry for it and the DohFlags hex means enabled:

	```registry
	[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\InterfaceSpecificParameters\{Interface-GUID}\DohProfileSettings\Doh\IPv4-ADDRESS]
	"DohFlags"=hex(b):11,00,00,00,00,00,00,00
	"Template"="DoH-URL"
	```

- **IPv6 should be recognized as Doh6:**

	```registry
	[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\InterfaceSpecificParameters\{Interface-GUID}\DohProfileSettings\Doh6]
	"Template"="DoH-URL"
	```

	Then entry for it and the DohFlags hex means enabled:

	```registry
	[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Dnscache\InterfaceSpecificParameters\{Interface-GUID}\DohProfileSettings\Doh6\IPv6-ADDRESS]
	"Template"="https://security.cloudflare-dns.com/dns-query"
	```

Replace `{Interface-GUID}` with your actual network adapter GUID. Replace `IPv4-ADDRESS` or `IPv6-ADDRESS` and `DoH-URL` with your values.

## License

MIT — see `LICENSE` for details.

---

Created by [1LUC1D4710N](https://github.com/1LUC1D4710N)

*Inspired by the spirit of Greenlandic tech and global privacy.*

## Why?

To make secure DNS configuration easy, flexible, and accessible for everyone.
# doh-registry-generator
