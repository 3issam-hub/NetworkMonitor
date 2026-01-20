<img src="NetM.png" alt="NetworkMonitor-logo" width="30%"/>
# Network Traffic Monitor - README

## Project Description

This Network Traffic Monitor is an advanced shell script that monitors network interface traffic in real-time and generates visual reports using Gnuplot. The system creates an auto-refreshing HTML page that displays traffic statistics and graphs.

## Features

### Core Features (Required)
1. ✅ **Interface Monitoring**: Monitor any network interface by name
2. ✅ **HTML Visualization**: Auto-refreshing HTML page with traffic graphs
3. ✅ **Gnuplot Integration**: Beautiful graphs showing RX/TX traffic over time
4. ✅ **Default Storage**: Results saved to `/tmp/[interface_name]/`

### Additional Features (Bonus)
1. ✅ **Configurable Interval**: Set custom monitoring intervals
2. ✅ **Custom Output Directory**: Specify where to save results
3. ✅ **Configurable Duration**: Set maximum monitoring window
4. ✅ **Zenity GUI**: Optional graphical interface for configuration
5. ✅ **CSV Export**: Export data to downloadable CSV files
6. ✅ **Comprehensive Statistics**: Display packets, errors, and dropped packets
7. ✅ **Modern UI**: Responsive, attractive HTML interface with auto-update
8. ✅ **Real-time Updates**: Live timestamp updates in the browser

## Prerequisites

### Required Software
- **Bash** (4.0 or higher)
- **Gnuplot** - For generating graphs
- **Linux** - Access to `/sys/class/net/` filesystem

### Optional Software
- **Zenity** - For GUI configuration (optional)

### Installation of Dependencies

#### On Debian/Ubuntu:
```bash
sudo apt-get update
sudo apt-get install gnuplot zenity
```

#### On Red Hat/CentOS/Fedora:
```bash
sudo yum install gnuplot zenity
```

#### On Arch Linux:
```bash
sudo pacman -S gnuplot zenity
```

## Installation

1. Download the script:
```bash
git clone https://github.com/3issam-hub/NetworkMonitor.git
```

2. Make it executable:
```bash
chmod +x network_monitor.sh
```

## Usage

### Basic Usage

Monitor the `eth0` interface with default settings:
```bash
./network_monitor.sh -i eth0
```

### Advanced Usage

#### Custom Interval and Duration
Monitor `wlan0` with 30-second intervals for 2 hours:
```bash
./network_monitor.sh -i wlan0 -t 30 -d 7200
```

#### Custom Output Directory
Save results to a web server directory:
```bash
./network_monitor.sh -i eth0 -o /var/www/html/monitor
```

#### With CSV Export
Export data to CSV file:
```bash
./network_monitor.sh -i eth0 -c
```

#### Using Zenity GUI
Launch with graphical configuration:
```bash
./network_monitor.sh -z
```

#### Complete Example
Full-featured monitoring:
```bash
./network_monitor.sh -i eth0 -t 60 -o /home/user/network_stats -d 3600 -c
```

### Command-Line Options

| Option | Long Form | Argument | Description |
|--------|-----------|----------|-------------|
| `-i` | `--interface` | IFACE | Network interface to monitor (required) |
| `-t` | `--interval` | SECONDS | Monitoring interval (default: 60) |
| `-o` | `--output` | DIR | Output directory (default: /tmp) |
| `-d` | `--duration` | SECONDS | Max monitoring duration (default: 3600) |
| `-z` | `--zenity` | - | Use Zenity GUI for configuration |
| `-c` | `--csv` | - | Export data to CSV file |
| `-h` | `--help` | - | Display help message |

## How It Works

### Data Collection
1. The script reads network statistics from `/sys/class/net/[interface]/statistics/`
2. It tracks both received (RX) and transmitted (TX) bytes
3. Every interval, it calculates the difference to determine bytes per minute
4. Data is stored in a CSV format for processing

### Visualization
1. **Gnuplot** processes the CSV data to create a PNG graph
2. The graph shows traffic over the last hour (configurable)
3. Separate lines for RX (blue) and TX (red) traffic
4. Data displayed in KB/min for readability

### HTML Page
1. Auto-refreshes every 60 seconds to show latest data
2. Displays current statistics from the network interface
3. Shows monitoring parameters and status
4. Provides download link for CSV data (if enabled)
5. Modern, responsive design works on desktop and mobile

### File Structure
When monitoring interface `eth0`, the following structure is created:
```
/tmp/eth0/
├── index.html              # Main HTML page
├── traffic_graph.png       # Generated graph
├── traffic_graph.png.gp    # Gnuplot script
├── traffic_data.csv        # Raw data (if CSV export enabled)
└── traffic_data.csv        # Internal data file
```

## Features Explanation

### What the Script Does

#### ✅ **Monitoring**
- Continuously monitors specified network interface
- Tracks bytes received and transmitted
- Calculates traffic per monitoring interval
- Maintains up to 1 hour of historical data

#### ✅ **Visualization**
- Generates high-quality PNG graphs with Gnuplot
- Shows traffic trends over time
- Color-coded RX (blue) and TX (red) lines
- Time-stamped data points

#### ✅ **Web Interface**
- Creates auto-refreshing HTML page
- Displays real-time statistics
- Shows monitoring configuration
- Provides visual traffic graph
- Modern, professional design

#### ✅ **Data Export**
- Optional CSV export for external analysis
- Download link in HTML page
- Compatible with Excel, LibreOffice, etc.

#### ✅ **Configuration**
- Command-line arguments for automation
- Zenity GUI for interactive setup
- Flexible interval and duration settings
- Custom output directory support

### What the Script Doesn't Do

#### ❌ **Limitations**
- Does not track individual connections or processes
- Does not filter by protocol (HTTP, FTP, etc.)
- Does not perform deep packet inspection
- Does not store data permanently (limited to max duration)
- Does not alert on threshold violations
- Does not support multiple interfaces simultaneously in a single instance
- Does not provide historical analysis beyond the monitoring window
- Does not require root privileges (reads from `/sys/class/net/`)

#### ❌ **Not Included**
- No database storage
- No email alerts
- No bandwidth throttling or QoS
- No network security monitoring
- No automatic report generation
- No web server (requires manual hosting if serving remotely)

## Examples

### Example 1: Monitor Home WiFi
```bash
./network_monitor.sh -i wlan0 -t 60 -o ~/network_monitor -c
```
Then open: `file:///home/username/network_monitor/wlan0/index.html`

### Example 2: Monitor Server Ethernet
```bash
./network_monitor.sh -i ens33 -t 30 -d 7200 -o /var/www/html/traffic
```
Then access via web server: `http://your-server/traffic/ens33/`

### Example 3: Quick Test
```bash
./network_monitor.sh -i eth0
```
View results: `file:///tmp/eth0/index.html`

### Example 4: Interactive Setup
```bash
./network_monitor.sh -z
```
Follow the GUI prompts to configure monitoring.

## Troubleshooting

### Common Issues

#### "Interface does not exist"
- Check available interfaces: `ls /sys/class/net/`
- Common names: `eth0`, `wlan0`, `ens33`, `enp0s3`
- Don't use `lo` (loopback interface)

#### "Missing dependencies: gnuplot"
- Install gnuplot: `sudo apt-get install gnuplot`

#### HTML page doesn't refresh
- Check browser settings (auto-refresh must be enabled)
- Try opening in a different browser
- Verify the meta refresh tag in HTML source

#### Graph not displaying
- Check if gnuplot installed: `which gnuplot`
- Verify PNG file exists: `ls -la /tmp/eth0/traffic_graph.png`
- Check gnuplot script for errors: `cat /tmp/eth0/traffic_graph.png.gp`

#### No data showing
- Verify interface is active: `ip link show [interface]`
- Check interface has traffic: `cat /sys/class/net/[interface]/statistics/rx_bytes`
- Wait for one monitoring interval to pass

#### Permission Denied
- Script doesn't need root for reading `/sys/class/net/`
- If using `/var/www/html`, ensure write permissions
- Use `-o ~/network_monitor` for home directory instead

### Debug Mode

To see detailed output, modify the script to add debug logging or run with verbose options:
```bash
bash -x ./network_monitor.sh -i eth0
```

## Viewing Results

### Local Viewing
Open the HTML file directly in a browser:
```bash
firefox /tmp/eth0/index.html
# or
google-chrome /tmp/eth0/index.html
# or
xdg-open /tmp/eth0/index.html
```

### Remote Viewing (via Web Server)
1. Copy output to web directory:
```bash
sudo ./network_monitor.sh -i eth0 -o /var/www/html/monitor -c
```

2. Access via browser:
```
http://your-server-ip/monitor/eth0/
```

## Technical Details

### Data Sources
- **RX Bytes**: `/sys/class/net/[iface]/statistics/rx_bytes`
- **TX Bytes**: `/sys/class/net/[iface]/statistics/tx_bytes`
- **RX Packets**: `/sys/class/net/[iface]/statistics/rx_packets`
- **TX Packets**: `/sys/class/net/[iface]/statistics/tx_packets`
- **Errors**: `/sys/class/net/[iface]/statistics/rx_errors` & `tx_errors`
- **Dropped**: `/sys/class/net/[iface]/statistics/rx_dropped` & `tx_dropped`

### Graph Format
- **Resolution**: 1200x600 pixels
- **Format**: PNG
- **Data Units**: KB/min (kilobytes per minute)
- **Time Range**: Last 60 minutes (configurable via duration)

### HTML Auto-Refresh
Uses meta tag: `<meta http-equiv="refresh" content="60">`
- Refreshes every 60 seconds
- Reloads entire page to show latest graph and statistics

## Script Architecture

### Modular Design
The script is organized into functions for maintainability:
- `print_usage()` - Help text
- `check_dependencies()` - Verify required programs
- `validate_interface()` - Check interface exists
- `get_network_stats()` - Read interface statistics
- `create_gnuplot_script()` - Generate Gnuplot commands
- `create_html_page()` - Build HTML report
- `monitor_traffic()` - Main monitoring loop
- `zenity_config()` - GUI configuration

### Error Handling
- Validates all inputs
- Checks dependencies before starting
- Graceful handling of missing interfaces
- Trap Ctrl+C for clean exit

## License

This project is provided as-is for educational purposes.

## Author

Network Monitor Project - Shell Scripting Mini-Project

## Version History

- **v1.0** - Initial release with all core and bonus features

## Support

For issues or questions:
1. Check the troubleshooting section
2. Verify prerequisites are installed
3. Review the examples for correct usage
4. Ensure network interface exists and is active

---

**Note**: This tool is designed for monitoring and visualization only. It does not perform any network modifications or security functions.
