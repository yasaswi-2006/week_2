
## Overview
On Day 0, Setting up the necessary tools for the SFAL-VSD-SoC-Design. This document outlines the installation steps for each tool required.

## System Details
- **RAM**: 16GB
- **Storage**: 256GB SSD
- **OS**: macOS

## Tools Setup

<ul>
    <li>
        <details>
            <summary>Yosys</summary>
            <p>Instructions:</p>
            <pre>
```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
$ git clone https://github.com/YosysHQ/yosys.git
$ brew install cmake gcc gawk tcl-tk libtool bison flex make
$ brew install graphviz
$ cd yosys
$ git submodule update --init
$ make
$ yosys --version
        </pre><img width="960" height="447" alt="Screenshot 2025-10-04 205957" src="https://github.com/user-attachments/assets/717a1567-15cf-4862-abdf-529da1adee07" />

    </details>
</li>
<li>
    <details>
        <summary>Iverilog</summary>
        <p>Instructions:</p>
        <pre>
$ brew install icarus-verilog
        </pre>
     <img width="960" height="457" alt="image" src="https://github.com/user-attachments/assets/6e32d3ab-b512-4dfb-9562-8454375ef277" />

    </details>
</li>
<li>
    <details>
        <summary>GTKWave</summary>
        <p>Instructions:</p>
        <pre>
$ brew install gtkwave
        </pre>
        <img width="960" height="459" alt="image" src="https://github.com/user-attachments/assets/9a713e69-7c0c-476a-adf6-7fc4cfa1fe0d" />

    </details>
</li>
</ul>
