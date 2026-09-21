[README.md](https://github.com/user-attachments/files/32453304/README.md)
# BloodHoundLite

A lightweight yet powerful BloodHound data analysis tool, enhanced for resource-constrained environments like Kali Linux with limited RAM and storage. This script allows you to load BloodHound JSON data (from SharpHound collectors), find attack paths, query nodes by properties, and visualize relationships.

## Features

-   **Data Ingestion:** Loads BloodHound data from `.json` files, directories of `.json` files, or `.zip` archives containing `.json` files.
-   **Attack Pathfinding:** Identifies the shortest attack path between a source and target node.
-   **Property-Based Node Querying:** Search for nodes based on their type, operating system, enabled status, or any other property present in the BloodHound data.
-   **Relationship Listing:** View all incoming and outgoing relationships for any given node, providing a quick overview of its connections.
-   **Interactive Visualization:** Generates an HTML report for attack paths, with nodes colored by type (Users: Blue, Groups: Green, Computers: Red) and detailed tooltips.

## System Requirements

-   **Operating System:** Designed for Linux distributions (like Kali Linux) but compatible with any OS that supports Python and its dependencies.
-   **RAM:** Minimum 2GB (5GB recommended for larger datasets).
-   **Storage:** Minimum 1GB free space for the script and generated reports (50GB total storage is more than sufficient).
-   **Python:** Python 3.x

## Installation

1.  **Download the Script:**
    You can access this script directly from your IDE or clone the GitHub repository once it's set up. Assuming you have the `bloodhound_lite.py` file:

    ```bash
    # Example: If you cloned the repository
    git clone https://github.com/Subir-T/bloodhound_lite.git
    cd bloodhound_lite
    ```

2.  **Install Dependencies:**
    Open your terminal and install the required Python packages:

    ```bash
    pip install networkx pyvis
    ```

## Usage

The script uses a command-line interface with different commands for various analysis tasks.

```bash
python3 bloodhound_lite.py <input_path> <command> [command_options]
```

-   `<input_path>`: Path to your BloodHound data. This can be a `.json` file, a directory containing `.json` files, or a `.zip` archive (e.g., `bloodhound_data.zip`, `data_folder/`).

### Commands:

1.  **`path` - Find an Attack Path**
    Finds the shortest path between a source and target node and generates an interactive HTML visualization.

    ```bash
    python3 bloodhound_lite.py bloodhound_data.zip path "S-1-5-21-..." "ADMINISTRATORS@DOMAIN.LOCAL" --output my_attack_path.html
    ```
    -   `start_node`: The starting node for the attack path (e.g., a user's SID or name).
    -   `target_node`: The target node for the attack path (e.g., a group or computer name).
    -   `--output <filename.html>` (optional): Specify the output HTML file for the path visualization. Defaults to `attack_path.html`.

2.  **`query` - Query Nodes by Property**
    Finds nodes that have a specific property with a given value.

    ```bash
    # Find all nodes of type 'User'
    python3 bloodhound_lite.py bloodhound_data.zip query type User

    # Find computers with a specific Operating System (case-insensitive by default)
    python3 bloodhound_lite.py bloodhound_data.zip query OperatingSystem "Windows Server 2019 Standard"

    # Find enabled users (case-sensitive search)
    python3 bloodhound_lite.py bloodhound_data.zip query Enabled True --case_sensitive
    ```
    -   `property_name`: The name of the node property (e.g., `type`, `OperatingSystem`, `Enabled`).
    -   `property_value`: The value to search for.
    -   `--case_sensitive` (optional): Use this flag for case-sensitive matching of the property value.

3.  **`relations` - List Node Relationships**
    Displays all incoming and outgoing relationships for a specified node.

    ```bash
    # List relationships for a specific user (by SID or Name)
    python3 bloodhound_lite.py bloodhound_data.zip relations "S-1-5-21-..."

    # List relationships for a specific group
    python3 bloodhound_lite.py bloodhound_data.zip relations "DOMAIN ADMINS@DOMAIN.LOCAL"
    ```
    -   `node_id`: The SID or name of the node you want to inspect.

---
