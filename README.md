# Holodeck
# Description: PowerCLI script to deploy or refresh a VMware Holodeck environment.
#              ----
# Reference: https://core.vmware.com/introducing-holodeck-toolkit
# Credit: VMware Holodeck Team
#         VMware Lab Configurator (VLC) Team - big shout out to Ben Sier
#         William Lam - huge thanks with my OVA import problem
#
# Changelog
# 01/17/24
#   * Initital draft
#   - kickoff initial or refresh HoloConsole ISO as needed while renaming prior version
#   - automate the creation/deletion of ESXi host network according to 'VMware Holo-Setup-Host-Prep' (January 2023)
#   - uploading custom Holo Console iso from Build Host to ESXi host datastore
#   - automate the deployment/deletion of Holo-Console VM according to 'VMware Holo-Setup-Deploy-Console' (January 2023)
#   - automated the deployment/deletion of Holo-Router according to 'VMware Holo-Setup-Deploy-Router' (January 2023)
#   - automate ESX host clean up to remove prior Holodeck deployment (i.e. HoloConsole, HoloRouter, Holodeck Network, Holodeck related ISO)
#   - Synchronize custom VLC configuration files between Build Host and HoloConsole VM (Manually)
# 
# To Do
#   - Kickoff VLC process in headless mode from Build Host or during newly ESX host refresh (WIP)
