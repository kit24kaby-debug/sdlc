# Currency Converter State Diagram

```mermaid
stateDiagram-v2
  [*] --> Idle
  Idle --> InputEntered: enter(amount, from, to)
  InputEntered --> InputInvalid: validateFail()
  InputInvalid --> Idle
  InputEntered --> InputValidated: validatePass()
  InputValidated --> Identity: from==to
  Identity --> Displayed: render()
  InputValidated --> RateLookup: FetchRate()
  RateLookup --> RateFromCache: hit && fresh
  RateLookup --> RateFromProvider: miss || stale
  RateFromProvider --> LookupFailed: providerError()
  LookupFailed --> Error: showError()
  Error --> Idle
  RateFromCache --> Computed: compute()
  RateFromProvider --> Computed: compute()
  Computed --> Displayed: render()
  Displayed --> Idle: reset() / newInput()