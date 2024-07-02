# Author: Dominic Chan
# Website: www.broadcom.com
# Description: PowerCLI script to deploy or refresh a VMware Holodeck environment.
#              ----
# Reference: https://core.vmware.com/introducing-holodeck-toolkit
# Credit: VMware Holodeck / VMware Lab Configurator Teams
#
# Changelog
# 07/02/24
#   * Update for Holodeck 5.1.1
#   - Autoextract Holodeck zip package
#   - Automate download require binaries based on Holodeck specification
#     * (manual download still requires for PowerCLI, OVFTools, CloudBuilder, and Aria)
#   - Update 'CreateISO.ps1' to match existing build environment and binaries versions based on Holodeck specification
#   - Update 'additionalfiles.txt' to match existing build environment and binaries versions
#   - Update 'additionalcommands.bat' to match existing build environment and binaries versions
#   - Update 'Holo-A1-511.ini' for VLC to match existing build environment and binaries versions
#
# 01/17/24
#   * Initital draft
#   - kickoff initial or refresh HoloConsole ISO as needed while renaming prior version
#   - automate the creation/deletion of ESXi host network according to 'VMware Holo-Setup-Host-Prep' (January 2023)
#   - uploading custom Holo Console iso from Build Host to ESXi host datastore
#   - automate the deployment/deletion of Holo-Console VM according to 'VMware Holo-Setup-Deploy-Console' (January 2023)
#   - automated the deployment/deletion of Holo-Router according to 'VMware Holo-Setup-Deploy-Router' (January 2023)
#   - automate ESX host clean up to remove prior Holodeck deployment (i.e. HoloConsole, HoloRouter, Holodeck Network, Holodeck related ISO)
#   - Synchronize custom VLC configuration files between Build Host and HoloConsole VM
#   - Kickoff VLC process in headless mode from Build Host 
# 
# To Do
#   - Headless VLC process for dual sites
#
# Required 
#   - Windows 10/11 Desktop with PowerShell (Optional: Visual Studio Code)
#   - VMware OVFTools install
#   - VMware ESX host with reasonable CPU cores, memory capacity, and storage
#   - List of require binaries from Broadcom / VMware support portal (see 'Note' section below)
#   - Minor edit/update to few (dozen) variables within this script that are marked with "UPDATE ACCORDINGLY TO YOUR LAB ENVIRONNMENT"
#       $VIServer - FQDN to your physical ESX host
#       $VIIP - Management IP address to your physical ESX host
#       $VIPassword - ROOT password to your physical ESX host
#       $labDNS = Lab DNS server with external DNS resolution (i.e. 8.8.8.8)
#       $HoloDeckPath - Path on your local desktop housing Holodeck and required binaries (min 250 GB)
#       $HoloDeckDS - ESX datastore for the automated nested VCF build (This also include any Aria components)
#       $HoloConsoleDS - ESX datastore to where Holodeck jumpbox (HoloConsole) would be deploy - can be the same $HoloDeckDS
#       $HoloRouterDS - ESX datastore to where Holodeck custom router (HoloRouter) would be deploy - can be the same $HoloDeckDS
#       $HoloConsoleISOName - Name to custom build ISO craeted by Holodeck for this automated build
#       $HoloRouterExternalIP = Static IP address dedicate for HoloRouter to faciliate all outbound VCF traffic (This is MUST to gain jumpbox access)
#       $HoloRouterExternalSubnet = Subnet mask from your lab environment
#       $HoloRouterExternalGW = IP address / Gateway for Internet access
#
# Note
#   The following binaries require a Broadcom / VMware account and must be download manually
#   - Latest VMware PowerCLI 13.2.1-22851661
#   - Latest VMware OVFTool 4.6.2 MSI
#   - VMware Cloud Foundation 5.1.1 Cloud Builder OVA
#   - VMware Aria Suite Lifecycle 8.16.0 Easy Installer for Aria Automation 8.16.2 ISO
