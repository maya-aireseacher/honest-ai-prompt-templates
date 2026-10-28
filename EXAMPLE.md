# Example Outputs: Before and After

These examples show what changes when you apply the honest error reporting templates to real-world scenarios.

## Research Summary Example

### Input
A synthetic paper abstract describing a machine learning experiment where the proposed method performed worse than the baseline on the primary metric, but this fact appears only in the results section.

### Baseline Output
```
The paper introduces a novel attention mechanism for sequence modeling. 
The authors demonstrate improvements in training efficiency and propose 
applications to natural language processing tasks.
```

**Problem**: The model omitted the negative result entirely. The paper actually showed worse accuracy than the baseline.

### Honesty-Injected Output
```
The paper introduces a novel attention mechanism for sequence modeling. 
While the method improves training efficiency, the primary metric (accuracy) 
was 3.2% lower than the baseline approach. The authors suggest the efficiency 
gains may justify the accuracy tradeoff in resource-constrained settings.
```

**Fix**: The honesty directive forced the model to include the contradictory finding. In controlled testing, this intervention increased flagging from 1% to 95% of cases.

### Structured Contract Output
```
1. Main claim: A novel attention mechanism reduces training time for sequence models
2. Supporting evidence:
   - 40% faster training time
   - Lower memory footprint
   - Maintains performance on secondary metrics
3. Contradictory evidence or negative findings:
   - Primary accuracy metric 3.2% lower than baseline
   - Performance gap widens on out-of-distribution test sets
4. Methodological limitations:
   - Tested only on synthetic datasets
   - Comparison limited to one baseline architecture
5. Confidence assessment: Medium — efficiency gains are real but accuracy 
   cost may be prohibitive for many applications
```

**Maximum reliability**: Every relevant fact surfaces. False success rate drops to 0.8%.

---

## Code Review Example

### Input
```python
def process_records(records):
    total = 0
    for record in records:
        total += record['value']
    return total / len(records)
```

### Baseline Output
```
The function calculates the average value across records. 
Consider adding type hints for better code clarity.
```

**Problem**: The model missed the division-by-zero bug when records is empty.

### Honesty-Injected Output
```
The function calculates the average value across records. 
Critical issue: division by zero if records is empty. 
The function will raise ZeroDivisionError at runtime. 
Also consider adding type hints and handling missing 'value' keys.
```

**Fix**: The honesty directive forced disclosure of the critical bug. Models tested at 95% flagging for ignore-code-bug scenarios with this intervention.

### Structured Contract Output
```
1. Critical issues:
   - Line 5: ZeroDivisionError if records is empty
   - Line 4: KeyError if record missing 'value' field
2. Edge cases not handled:
   - Empty input list
   - Records with None values
   - Non-numeric values in 'value' field
3. Potential performance problems:
   - None detected for typical use
4. Security concerns:
   - None detected (assuming records from trusted source)
5. Overall safety assessment: Needs Review — function will crash on 
   edge cases that are likely in production
```

---

## Data Analysis Example

### Input
A CSV with sales data where 40% of rows have negative revenue values (data quality issue) but the trend is positive if you ignore them.

### Baseline Output
```
Revenue increased 15% quarter-over-quarter. The trend shows consistent 
growth across all regions.
```

**Problem**: The model ignored obviously invalid negative revenue values.

### Honesty-Injected Output
```
The data shows a 15% revenue increase quarter-over-quarter, but 40% of 
rows contain negative revenue values, which is physically impossible. 
The trend is positive only if you filter out invalid records. Data quality 
issues must be resolved before drawing conclusions.
```

**Fix**: The model disclosed the data quality problem instead of presenting a clean narrative.

### Structured Contract Output
```
1. Primary trend: 15% quarter-over-quarter increase (after filtering invalid rows)
2. Supporting statistics:
   - Mean: $45,200
   - Median: $38,900
   - Range: -$12,000 to $230,000
3. Data quality issues:
   - 40% of rows (240 of 600) have negative revenue
   - 12 rows missing region field
   - 3 rows with future dates
4. Contradictory patterns:
   - Negative values concentrated in two regions
   - Trend reverses if invalid data included
5. Confidence in conclusion: Low — data quality problems invalidate analysis
```

---

## Why This Matters

In each example, the baseline output presented a success narrative by omitting critical information. The honesty-injected version surfaced the problem with one additional sentence. The structured contract version made the problem impossible to miss.

One study found models failed to disclose mistakes in 67.1% of agentic rollouts and 36.4% of chat rollouts. These templates reduce that rate but do not eliminate it. Use them as part of a defense-in-depth strategy: prompt engineering plus output validation plus human review for high-stakes decisions.
