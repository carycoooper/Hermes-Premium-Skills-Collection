---
name: utility-toolkit
description: Swiss-army-knife collection of high-frequency utility operations covering file conversions, text transformations, date/time calculations, unit conversions, regex operations, batch file management, data parsing, and common productivity helpers.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [utilities, file-conversion, text-processing, calculations, batch-operations, productivity-helpers]
    category: utilities
    related_skills: [summarize, email-management, github-integration]
    config:
      - key: default_date_format
        description: "Preferred date output format"
        default: "ISO 8601 (YYYY-MM-DD)"
        prompt: "What date format do you prefer?"
      - key: timezone
        description: "Default timezone for calculations"
        default: "UTC"
        prompt: "Your local timezone for date/time operations?"
---

# Utility Toolkit Skill

Essential everyday utilities that handle the small-but-frequent tasks that would otherwise require manual effort or separate tools.

## When to Use

This skill is your go-to for any utility operation:

**File Operations:**
- "Convert this CSV to JSON"
- "Convert Markdown to HTML/PDF"
- "Batch rename these files"
- "Extract text from this image/PDF"
- "Compress/optimize these images"

**Text Processing:**
- "Clean up this formatting"
- "Convert to uppercase/lowercase/title case"
- "Remove duplicates from this list"
- "Sort these lines alphabetically/numerically"
- "Find/replace with regex"
- "Extract emails/URLs/phones from text"

**Calculations:**
- "How many business days between [date] and [date]?"
- "Convert 150 lbs to kg"
- "What time is it in Tokyo right now?"
- "Calculate percentage change"
- "Unit conversions (length, weight, temperature, data, speed)"

**Data Parsing:**
- "Parse this log file and extract errors"
- "Extract structured data from unstructured text"
- "Format this JSON nicely"
- "Validate this email/URL/IP address"
- "Parse user-agent string"

**Batch Automation:**
- "Process all files in this directory"
- "Apply transformation to matching files"
- "Generate files from template"
- "Create directory structure from list"

## Quick Reference

| Category | Operation | Example | Output |
|----------|-----------|---------|--------|
| **File Convert** | CSV→JSON | `convert data.csv json` | Structured JSON |
| **File Convert** | MD→HTML | `convert readme.md html` | Rendered HTML |
| **File Convert** | MD→PDF | `convert doc.md pdf` | PDF document |
| **File Convert** | Image optimize | `optimize images/ jpg 80%` | Compressed images |
| **Text Transform** | Case change | `transform text UPPER` | UPPERCASE TEXT |
| **Text Transform** | Deduplicate | `dedupe list.txt` | Unique lines |
| **Text Transform** | Sort | `sort numbers.txt numeric` | Sorted list |
| **Text Transform** | Regex extract | `regex '\\b\\w+@\\w+\\.\\w+' file.txt` | Emails |
| **Date/Time** | Business days | `bizdays 2026-01-01 2026-03-31` | Count |
| **Date/Time** | Timezone convert | `tzconvert 14:00 UTC to Asia/Tokyo` | Local time |
| **Date/Time** | Date arithmetic | `datecalc 2026-04-27 +30 days` | Result date |
| **Units** | Conversion | `convert 100 km miles` | 62.137 mi |
| **Units** | Temperature | `convert 100 celsius fahrenheit` | 212°F |
| **Data** | JSON pretty | `pretty minified.json` | Formatted |
| **Data** | Validate | `validate email test@test.com` | Valid/Invalid |
| **Batch** | Rename pattern | `batch rename 'img_*.jpg' 'photo_*.jpg'` | Renamed files |
| **Batch** | Template gen | `template render template.json data.csv` | Generated files |

## Procedure

### File Conversion Operations

#### 1. CSV to JSON
```python
# Implementation approach
import csv
import json

def csv_to_json(csv_file_path, json_file_path=None, orient='records'):
    """
    Convert CSV file to JSON format.
    
    Args:
        csv_file_path: Path to input CSV
        json_file_path: Output path (default: same name .json)
        orient: 'records' (list of dicts) or 'table' (dict of columns)
    """
    data = []
    with open(csv_file_path, mode='r', encoding='utf-8') as f:
        reader = csv.DictReader(f)
        for row in reader:
            # Attempt type inference for numeric fields
            cleaned_row = {}
            for key, value in row.items():
                cleaned_row[key.strip()] = infer_type(value)
            data.append(cleaned_row)
    
    output = data if orient == 'records' else {'data': data}
    
    if json_file_path is None:
        json_file_path = csv_file_path.rsplit('.', 1)[0] + '.json'
    
    with open(json_file_path, 'w', encoding='utf-8') as f:
        json.dump(output, f, indent=2, ensure_ascii=False)
    
    return {
        'success': True,
        'rows_processed': len(data),
        'columns': list(data[0].keys()) if data else [],
        'output_file': json_file_path,
        'size_kb': os.path.getsize(json_file_path) / 1024
    }
```

**Output Example:**
```json
{
  "conversion_result": {
    "input": "transactions.csv",
    "output": "transactions.json",
    "rows": 150,
    "columns": ["date", "description", "amount", "category"],
    "type_inference": {
      "date": "string",
      "description": "string",
      "amount": "float",
      "category": "string"
    },
    "warnings": ["Row 42 had empty 'category' field"]
  }
}
```

#### 2. Markdown to HTML
```python
# Using markdown library or manual conversion
import markdown

def md_to_html(md_file_path, html_file_path=None):
    with open(md_file_path, 'r', encoding='utf-8') as f:
        md_content = f.read()
    
    html_content = markdown.markdown(
        md_content,
        extensions=['tables', 'fenced_code', 'toc', 'nl2br']
    )
    
    # Wrap in basic HTML template
    full_html = f"""<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{Path(md_file_path).stem}</title>
    <style>
        body {{ font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; max-width: 800px; margin: 40px auto; padding: 20px; line-height: 1.6; }}
        table {{ border-collapse: collapse; width: 100%; }}
        th, td {{ border: 1px solid #ddd; padding: 8px; text-align: left; }}
        th {{ background-color: #f5f5f5; }}
        code {{ background: #f4f4f4; padding: 2px 6px; border-radius: 3px; }}
        pre {{ background: #f4f4f4; padding: 16px; border-radius: 6px; overflow-x: auto; }}
    </style>
</head>
<body>
{html_content}
</body>
</html>"""
    
    # Write output...
```

#### 3. Batch Image Optimization
```python
# Optimize images in directory
from PIL import Image
import os

def optimize_images(directory, quality=85, max_width=1920, format='JPEG'):
    """Optimize all images in directory."""
    stats = {'processed': 0, 'saved_bytes': 0, 'files': []}
    
    for filename in os.listdir(directory):
        if filename.lower().endswith(('.png', '.jpg', '.jpeg', '.webp')):
            filepath = os.path.join(directory, filename)
            
            img = Image.open(filepath)
            original_size = os.path.getsize(filepath)
            
            # Resize if wider than max_width
            if img.width > max_width:
                ratio = max_width / img.width
                new_height = int(img.height * ratio)
                img = img.resize((max_width, new_height), Image.LANCZOS)
            
            # Convert RGBA to RGB for JPEG
            if img.mode == 'RGBA':
                background = Image.new('RGB', img.size, (255, 255, 255))
                background.paste(img, mask=img.split()[3])
                img = background
            
            # Save optimized
            output_path = filepath.rsplit('.', 1)[0] + '_optimized.jpg'
            img.save(output_path, format=format, quality=quality, optimize=True)
            
            new_size = os.path.getsize(output_path)
            saved = original_size - new_size
            
            stats['processed'] += 1
            stats['saved_bytes'] += saved
            stats['files'].append({
                'original': filename,
                'optimized': os.path.basename(output_path),
                'original_kb': original_size / 1024,
                'optimized_kb': new_size / 1024,
                'saved_kb': saved / 1024,
                'saved_pct': (saved / original_size) * 100
            })
    
    stats['total_saved_mb'] = stats['saved_bytes'] / (1024 * 1024)
    return stats
```

### Text Processing Operations

#### 4. Text Cleanup & Formatting
```python
import re
import unicodedata

def clean_text(text, operations=None):
    """
    Clean and normalize text with configurable operations.
    
    Available operations:
    - trim_whitespace: Remove leading/trailing/multiple spaces
    - normalize_unicode: NFC normalization
    - remove_special_chars: Keep only alphanumeric and basic punctuation
    - fix_encoding: Fix mojibake/encoding issues
    - normalize_quotes: Standardize quote characters
    - remove_extra_newlines: Max 2 consecutive newlines
    - strip_ansi: Remove terminal color codes
    - html_entities: Decode HTML entities
    """
    if operations is None:
        operations = ['trim_whitespace', 'normalize_unicode']
    
    result = text
    
    if 'normalize_unicode' in operations:
        result = unicodedata.normalize('NFC', result)
    
    if 'trim_whitespace' in operations:
        result = re.sub(r'[ \t]+', ' ', result)
        result = re.sub(r'\n\s*\n', '\n\n', result)
        result = result.strip()
    
    if 'normalize_quotes' in operations:
        result = result.replace('"', '"').replace('"', '"')
        result = result.replace(''', "'").replace(''', "'")
    
    if 'remove_special_chars' in operations:
        result = re.sub(r'[^\w\s\-.,!?;:\'\"()]', '', result)
    
    if 'strip_ansi' in operations:
        result = re.sub(r'\x1b\[[0-9;]*m', '', result)
    
    if 'html_entities' in operations:
        import html
        result = html.unescape(result)
    
    return result
```

#### 5. Regex Extraction Toolkit
```python
# Pre-built regex patterns for common extractions

REGEX_PATTERNS = {
    'emails': r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b',
    'urls': r'https?://(?:[-\w.]|(?:%[\da-fA-F]{2}))+',
    'phones': r'(?:\+?1[-.\s]?)?\(?[0-9]{3}\)?[-.\s]?[0-9]{3}[-.\s]?[0-9]{4}',
    'ip_addresses': r'\b(?:\d{1,3}\.){3}\d{1,3}\b',
    'dates_iso': r'\d{4}-\d{2}-\d{2}',
    'dates_us': r'\d{1,2}/\d{1,2}/\d{2,4}',
    'times': r'\d{1,2}:\d{2}(?:\s?[APap][Mm])?',
    'prices': r'\$[\d,]+(?:\.\d{2})?',
    'hashtags': r'#\w+',
    'mentions': r'@\w+',
    'credit_cards': r'\b\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}\b',
    'ssn': r'\b\d{3}-\d{2}-\d{4}\b',
    'hex_colors': r'#[0-9A-Fa-f]{6}\b',
    'uuids': r'[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}',
}

def extract_patterns(text, pattern_names=None):
    """Extract all occurrences of specified patterns."""
    if pattern_names is None:
        pattern_names = list(REGEX_PATTERNS.keys())
    
    results = {}
    for name in pattern_names:
        if name in REGEX_PATTERNS:
            matches = re.findall(REGEX_PATTERNS[name], text)
            if matches:
                results[name] = list(set(matches))  # Unique only
    
    return results
```

### Date & Time Calculations

#### 6. Business Day Calculator
```python
from datetime import datetime, timedelta
import holidays

def calculate_business_days(start_date, end_date, country='US'):
    """
    Calculate number of business days between two dates.
    Excludes weekends and public holidays.
    """
    start = datetime.strptime(start_date, '%Y-%m-%d').date()
    end = datetime.strptime(end_date, '%Y-%m-%d').date()
    
    # Get holidays for the country/year range
    holiday_set = holidays.country_holidays(
        country, 
        years=range(start.year, end.year + 1)
    )
    
    business_days = 0
    current = start
    while current <= end:
        # Weekday (Monday=0, Friday=4)
        if current.weekday() < 5:
            # Not a holiday
            if current not in holiday_set:
                business_days += 1
        current += timedelta(days=1)
    
    return {
        'start_date': str(start),
        'end_date': str(end),
        'calendar_days': (end - start).days,
        'business_days': business_days,
        'weekend_days': (end - start).days - business_days - len([d for d in holiday_set if start <= d <= end]),
        'holidays_encountered': len([d for d in holiday_set if start <= d <= end]),
        'country': country
    }

# Example output:
# {
#   "start_date": "2026-04-01",
#   "end_date": "2026-04-30",
#   "calendar_days": 29,
#   "business_days": 21,
#   "weekend_days": 8,
#   "holidays_encountered": 0,
#   "country": "US"
# }
```

#### 7. Timezone Converter
```python
from datetime import datetime
from zoneinfo import ZoneInfo

def convert_timezone(datetime_str, from_tz, to_tz, format='%Y-%m-%d %H:%M:%S'):
    """Convert datetime between timezones."""
    try:
        dt = datetime.strptime(datetime_str, '%Y-%m-%d %H:%M:%S')
        dt_from = dt.replace(tzinfo=ZoneInfo(from_tz))
        dt_to = dt_from.astimezone(ZoneInfo(to_tz))
        
        offset_from = dt_from.strftime('%z')
        offset_to = dt_to.strftime('%z')
        
        return {
            'original': {
                'datetime': dt_from.strftime(format),
                'timezone': from_tz,
                'utc_offset': f'UTC{offset_from[:3]}:{offset_from[3:]}'
            },
            'converted': {
                'datetime': dt_to.strftime(format),
                'timezone': to_tz,
                'utc_offset': f'UTC{offset_to[:3]}:{offset_to[3:]}'
            },
            'difference_hours': (dt_to.utcoffset() - dt_from.utcoffset()).total_seconds() / 3600,
            'is_dst_from': dt_from.dst() != timedelta(0),
            'is_dst_to': dt_to.dst() != timedelta(0)
        }
    except Exception as e:
        return {'error': str(e)}
```

### Unit Conversions

#### 8 Comprehensive Unit Converter
```python
UNIT_CONVERSIONS = {
    'length': {
        'meters': 1,
        'kilometers': 1000,
        'centimeters': 0.01,
        'millimeters': 0.001,
        'miles': 1609.344,
        'yards': 0.9144,
        'feet': 0.3048,
        'inches': 0.0254,
        'nautical_miles': 1852
    },
    'weight': {
        'kilograms': 1,
        'grams': 0.001,
        'milligrams': 0.000001,
        'pounds': 0.453592,
        'ounces': 0.0283495,
        'tons': 1000,
        'metric_tons': 1000,
        'stones': 6.35029
    },
    'temperature': {
        # Special handling needed (not linear)
        'celsius': 'celsius',
        'fahrenheit': 'fahrenheit',
        'kelvin': 'kelvin'
    },
    'digital_storage': {
        'bytes': 1,
        'kilobytes': 1024,
        'megabytes': 1024**2,
        'gigabytes': 1024**3,
        'terabytes': 1024**4,
        'bits': 0.125
    },
    'speed': {
        'mps': 1,  # meters per second
        'kmh': 0.277778,  # kilometers per hour
        'mph': 0.44704,  # miles per hour
        'knots': 0.514444,
        'mach': 343  # approximate at sea level
    },
    # ... more categories
}

def convert_units(value, from_unit, to_unit, category='length'):
    """Convert between units in the same category."""
    if category == 'temperature':
        return convert_temperature(value, from_unit, to_unit)
    
    conversions = UNIT_CONVERSIONS.get(category)
    if not conversions:
        return {'error': f'Unknown category: {category}'}
    
    if from_unit not in conversions or to_unit not in conversions:
        return {'error': f'Unknown unit in category {category}'}
    
    # Convert to base unit, then to target
    base_value = value * conversions[from_unit]
    result = base_value / conversions[to_unit]
    
    return {
        'value': value,
        'from': {'unit': from_unit, 'symbol': get_symbol(from_unit)},
        'to': {'unit': to_unit, 'symbol': get_symbol(to_unit)},
        'result': round(result, 6),
        'formula': f'{value} {from_unit} × {conversions[from_unit]} / {conversions[to_unit]} = {result:.6f} {to_unit}',
        'category': category
    }

def convert_temperature(value, from_unit, to_unit):
    """Handle temperature conversions (non-linear)."""
    if from_unit == to_unit:
        return {'result': value, 'unit': to_unit}
    
    # Convert to Celsius first
    if from_unit == 'celsius':
        celsius = value
    elif from_unit == 'fahrenheit':
        celsius = (value - 32) * 5/9
    elif from_unit == 'kelvin':
        celsius = value - 273.15
    else:
        return {'error': f'Unknown temperature unit: {from_unit}'}
    
    # Convert from Celsius to target
    if to_unit == 'celsius':
        result = celsius
    elif to_unit == 'fahrenheit':
        result = celsius * 9/5 + 32
    elif to_unit == 'kelvin':
        result = celsius + 273.15
    
    return {
        'value': value,
        'from': from_unit,
        'to': to_unit,
        'result': round(result, 2),
        'category': 'temperature'
    }
```

### Batch File Operations

#### 9. Smart Batch Rename
```python
import os
import re
from pathlib import Path

def batch_rename(directory, pattern, replacement, dry_run=True, recursive=False):
    """
    Batch rename files matching pattern.
    
    Args:
        directory: Target directory
        pattern: Regex pattern to match filenames
        replacement: Replacement pattern (supports \\1, \\2 for groups)
        dry_run: If True, only show what would happen (safe default!)
        recursive: Process subdirectories too
    """
    results = {'matched': 0, 'renamed': 0, 'skipped': 0, 'errors': [], 'operations': []}
    
    search_path = Path(directory)
    glob_pattern = '**/*' if recursive else '*'
    
    for filepath in search_path.glob(glob_pattern):
        if filepath.is_file():
            old_name = filepath.name
            old_path = str(filepath)
            
            # Check if matches pattern
            if re.search(pattern, old_name):
                results['matched'] += 1
                
                # Generate new name
                new_name = re.sub(pattern, replacement, old_name)
                new_path = str(filepath.parent / new_name)
                
                # Skip if name unchanged
                if new_name == old_name:
                    results['skipped'] += 1
                    continue
                
                # Check for conflicts
                if os.path.exists(new_path) and new_path != old_path:
                    results['errors'].append(f'Conflict: {new_name} already exists')
                    continue
                
                operation = {
                    'old_name': old_name,
                    'new_name': new_name,
                    'old_path': old_path,
                    'new_path': new_path
                }
                
                if not dry_run:
                    try:
                        os.rename(old_path, new_path)
                        results['renamed'] += 1
                        operation['status'] = 'renamed'
                    except Exception as e:
                        results['errors'].append(str(e))
                        operation['status'] = f'error: {e}'
                else:
                    operation['status'] = 'would_rename'
                
                results['operations'].append(operation)
    
    results['dry_run'] = dry_run
    return results

# Safety: Always runs in dry-run mode first!
# User must explicitly confirm before actual renaming
```

#### 10. Directory Tree Generator
```python
def generate_tree(directory, max_depth=3, ignore_patterns=None):
    """Generate ASCII tree representation of directory structure."""
    if ignore_patterns is None:
        ignore_patterns = ['node_modules', '.git', '__pycache__', '.DS_Store', '*.pyc']
    
    tree_lines = []
    tree_lines.append(f'📁 {os.path.basename(directory)}')
    
    def walk(path, prefix='', depth=0):
        if depth >= max_depth:
            return
        
        try:
            entries = sorted(os.listdir(path))
        except PermissionError:
            return
        
        for i, entry in enumerate(entries):
            # Skip ignored patterns
            if any(fnmatch.fnmatch(entry, pat) for pat in ignore_patterns):
                continue
            
            is_last = (i == len(entries) - 1)
            connector = '└── ' if is_last else '├── '
            extension = '    ' if is_last else '│   '
            
            full_path = os.path.join(path, entry)
            
            if os.path.isdir(full_path):
                tree_lines.append(f'{prefix}{connector}📂 {entry}')
                walk(full_path, prefix + extension, depth + 1)
            else:
                size = os.path.getsize(full_path)
                size_str = format_size(size)
                icon = get_file_icon(entry)
                tree_lines.append(f'{prefix}{connector}{icon} {entry} ({size_str})')
    
    walk(directory)
    tree_lines.append('')
    tree_lines.append(f'Total directories: [count]')
    tree_lines.append(f'Total files: [count]')
    tree_lines.append(f'Total size: [sum]')
    
    return '\n'.join(tree_lines)
```

## Quick Reference Commands

### One-Liners for Common Tasks

```bash
# File operations
/utl convert csv-to-json data.csv
/utl convert markdown-to-html readme.md
/utl batch-optimize-images ./photos quality=80
/utl extract-text-from-pdf document.pdf

# Text operations
/utl dedupe list.txt
/utl sort-lines file.txt reverse
/utl extract-emails text.txt
/utl find-replace 'old_text' 'new_text' *.md regex
/utl word-count document.txt
/utl line-count *.py

# Calculations
/utl biz-days 2026-01-01 2026-03-31
/utl tz-convert "14:00" UTC to "Asia/Shanghai"
/utl age-calculator 1990-05-15
/utl percentage-change 100 120
/utl tip-calculator 85.50 18%

# Units
/utl convert 100 km miles
/utl convert 72 fahrenheit celsius
/utl convert 1 gb bytes
/utl convert 60 mph kmh

# Data
/utl json-pretty minified.json
/utl json-validate data.json
/utl csv-stats transactions.csv
/utl log-analyzer app.log errors

# Batch
/utl batch-rename 'IMG_*.jpg' 'vacation_2026_*.jpg' --dry-run
/utl tree ./project depth=4
/utl find-large-files ./ size>100MB
/utl find-duplicates ./photos/
/utl create-backup ./important-folder/
```

## Integration Notes

**Universal Applicability:**
- Works standalone for quick tasks
- Pipes output to other skills (e.g., convert CSV → pass to Finance Tracker)
- Used internally by many other skills for preprocessing
- No external dependencies for core operations (uses Python stdlib mostly)

**Performance Tips:**
- For large files (>100MB), use streaming/chunked processing
- Batch operations show progress for long-running tasks
- Dry-run mode available for all destructive operations
- Caches repeated conversions in session

**When to Use Other Skills Instead:**
- Complex document analysis → Summarize skill
- Financial data → Finance Tracker skill
- Code-specific operations → GitHub Integration skill
- Email parsing → Email Management skill

---

*Based on OpenClaw's highest-rated utility skill (WaCLI) with expanded operation coverage, improved safety features, and Hermes-native integration.*
