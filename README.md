# Memory Allocation Simulator

A comprehensive desktop application for simulating contiguous memory allocation techniques with a professional GUI built using Python and Tkinter.

## Overview

This application simulates memory management using three different allocation strategies:
- **First Fit**: Allocates memory in the first available block that is large enough
- **Best Fit**: Allocates memory in the smallest available block that fits the process
- **Worst Fit**: Allocates memory in the largest available block

The simulator analyzes and visualizes **internal and external fragmentation** for different process sizes.

## Features

### Core Features
✅ Three memory allocation strategies (First Fit, Best Fit, Worst Fit)
✅ Visual memory layout representation with color-coded blocks
✅ Process allocation and deallocation
✅ Fragmentation analysis (internal & external)
✅ Memory statistics and utilization metrics
✅ Process history and block management
✅ Dynamic memory visualization
✅ Automatic block merging/compaction

### User Interface
- Interactive GTK-style interface with professional design
- Real-time memory visualization with color-coded blocks
- Process management table with details
- Comprehensive statistics display
- Detailed fragmentation analysis reports
- Support for random process allocations

## Project Structure

```
memory-allocation-simulator/
├── main.py                 # Application entry point
├── requirements.txt        # Python dependencies
├── README.md              # This file
│
├── backend/
│   ├── __init__.py        # Package initialization
│   ├── memory.py          # Memory management system
│   │                       ├── MemoryBlock: Block representation
│   │                       └── Memory: Main memory manager
│   └── analyzer.py        # Fragmentation analysis
│                           └── FragmentationAnalyzer: Analysis tools
│
└── ui/
    ├── __init__.py        # Package initialization
    ├── gui.py             # Main GUI application
    │                       └── MemoryAllocationSimulator: Main window
    └── visualizer.py      # Memory visualization
                            ├── MemoryVisualizer: Block visualization
                            └── StatisticsDisplay: Stats formatting
```

## Installation & Usage

### Prerequisites
- Python 3.6+
- tkinter (usually comes with Python)

### Installation

1. Navigate to the project directory:
```bash
cd S:\Edu\BTIT\OS\Project
```

2. (Optional) Install dependencies:
```bash
pip install -r requirements.txt
```

### Running the Application

```bash
python main.py
```

## How to Use

### 1. Select Allocation Strategy
Choose one of three strategies from the left panel:
- **First Fit**: Quick allocation in the first suitable block
- **Best Fit**: Minimizes wasted space in allocation blocks
- **Worst Fit**: Attempts to reduce fragmentation

### 2. Allocate Memory
- Enter the process size (in KB) in the input field
- Click **"Allocate"** to allocate memory using the selected strategy
- Or click **"Random Allocate"** to allocate a random-sized process (50-300 KB)

### 3. Monitor Memory
The visualization shows:
- **Color-coded blocks**: Each process has a unique color
- **Free blocks**: Shown in cyan/turquoise
- **Block information**: Process ID and size displayed on each block
- **Address scale**: Memory addresses shown at the bottom

### 4. Deallocate Memory
1. Select a process from the "Allocated Processes" table
2. Click **"Deallocate"** to free that memory
3. Adjacent free blocks are automatically merged

### 5. View Statistics
Real-time statistics display:
- Total, allocated, and free memory
- Memory utilization percentage
- External fragmentation percentage
- Largest contiguous free block size
- Number of allocated and free blocks

### 6. Generate Reports
Click **"Generate Report"** to get a detailed fragmentation analysis report showing:
- Memory distribution
- Fragmentation metrics
- Block statistics

### 7. Reset Memory
Click **"Reset Memory"** to clear all allocations and start fresh.

## Technical Details

### Memory Management Algorithm

```
Memory Block Structure:
┌─────────────────────────────────────┐
│ Block ID | Start | Size | Allocated│
└─────────────────────────────────────┘

Example: Block(0, Start:0, Size:500, Free)
```

### First Fit Algorithm
```
1. Scan memory from beginning
2. Find first block with size >= requested_size
3. Allocate in that block
4. Split block if larger than needed
```

### Best Fit Algorithm
```
1. Scan all free blocks
2. Find block with minimum size >= requested_size
3. Allocate in that block
4. Split block if larger than needed
```

### Worst Fit Algorithm
```
1. Scan all free blocks
2. Find block with maximum size
3. Allocate in that block
4. Split block if larger than needed
```

### Fragmentation Analysis

**External Fragmentation**:
- Formula: `(Total Free Space - Largest Contiguous Block) / Total Free Space × 100`
- High values indicate many small scattered free blocks

**Internal Fragmentation**:
- In this implementation: 0% (exact allocation with no wasted space within blocks)
- In real systems: occurs when allocated block is larger than needed

**Memory Utilization**:
- Formula: `(Total Allocated Space) / (Total Memory Size) × 100`
- Shows how much of total memory is actually in use

## Example Scenarios

### Scenario 1: First Fit vs Best Fit
```
Initial State: 1000 KB free

Allocation 1: 50 KB
├ First Fit:  Block starts at 0 KB
└ Best Fit:   Block starts at 0 KB (same)

Allocation 2: 100 KB
├ First Fit:  Block starts at 50 KB
└ Best Fit:   Block starts at 50 KB (same)

Allocation 3: 30 KB
├ First Fit:  Block starts at 150 KB
└ Best Fit:   Block starts at 150 KB (same)

After Deallocation of 100 KB block:
├ First Fit:  Next allocation uses the 100 KB hole at 50 KB
└ Best Fit:   Next allocation uses the 100 KB hole at 50 KB
```

### Scenario 2: Fragmentation Comparison

When you rapidly allocate and deallocate different-sized processes:
- **First Fit**: May create more fragmentation due to allocation strategy
- **Best Fit**: Tends to create less waste initially but may cause more complex patterns
- **Worst Fit**: Attempts to balance utilization across larger blocks

## Performance Notes

- Memory size: 1000 KB (configurable in code)
- Suitable for educational purposes
- Real-time visualization updates
- Block merging prevents worst-case fragmentation

## Extending the Project

To modify or extend the simulator:

### Change Total Memory Size
In [main.py](main.py), modify the Memory initialization:
```python
self.memory = Memory(total_memory_size=2000)  # Default is 1000
```

### Add New Allocation Strategy
1. In [backend/memory.py](backend/memory.py), add new method in `Memory` class
2. Update `_find_suitable_block()` to handle new strategy
3. Add radio button in [ui/gui.py](ui/gui.py)

### Custom Block Colors
Edit the `colors` dictionary in [ui/visualizer.py](ui/visualizer.py)

## Algorithm Comparison

| Strategy | Pros | Cons |
|----------|------|------|
| **First Fit** | Fast allocation, low overhead | May cause fragmentation |
| **Best Fit** | Minimizes wasted space | Slower, more fragmentation |
| **Worst Fit** | Reduces fragmentation | Less effective than theory suggests |

## Learning Outcomes

This simulator helps understand:
- Memory management in operating systems
- Fragmentation and its impact on performance
- Tradeoffs between allocation strategies
- Visualization of abstract OS concepts
- Python GUI development with tkinter

## Troubleshooting

### Application won't start
- Ensure Python 3.6+ is installed
- Check that tkinter is available: `python -m tkinter`

### Visualization not updating
- Click "Reset Memory" to refresh
- Resize the window to trigger canvas redraw

### Import errors
- Ensure you're running from the project directory
- Check that [backend/](backend/) and [ui/](ui/) directories exist with __init__.py files

## License

This project is for educational purposes.

## Author

Created as an Operating Systems course project

## References

- OS Memory Management Concepts
- Contiguous Memory Allocation
- Memory Fragmentation Analysis
- Python tkinter Documentation

---

**Last Updated**: February 2026

For questions or improvements, feel free to modify and extend the code!
