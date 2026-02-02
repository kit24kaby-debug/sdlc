# Existing State Diagram

<!-- Existing state diagram content goes here -->

## Currency Converter Flowchart

flowchart TD
  S([Start]) --> V{Valid input? (amount>0, supported codes)}
  V -- No --> ERR[Show validation error] --> Idle([Idle])
  V -- Yes --> Same{from == to?}
  Same -- Yes --> Identity[Return amount unchanged] --> Disp[Display result + timestamp + source] --> Next{Reset / New input?} -->|Yes| S
  Same -- No --> Hit{Cache hit & fresh?}
  Hit -- Yes --> Compute[Compute amount × rate] --> Disp
  Hit -- No --> Fetch[Fetch base rates from provider] --> Derive[Derive pair rate] --> Cache[Update cache] --> Compute --> Disp
  Fetch -.fail.-> ProvErr[Provider error] --> ERR --> Idle
  Disp -- No --> End([End])