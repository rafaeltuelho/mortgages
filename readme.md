# Drools Mortgages Rules Demo Project

This quickstart project provides a sample use case with different Rules definitions using a Mortgage Domain Model.

## Content

The following rule assets are defined:
 * Domain Model (Java Pojos)
 * Guided Rules
 * Guided Rules (with DSL)
   * Domain Specific Language (DSL)
 * Drools Rule Language (DRL)
 * Enumeration
 * Test Scenarios
   * Legacy (`.scenario`)
   * New (`.scesim`) - with `ScenarioJunitActivator` to execute tests with Maven.

## Testing

After building the `kjar` artifact and deploying into a kie-server runtime you can test the project rules using the rest API:

POST to 
   * JBoss based Kie-Server `/server/containers/instances/mortgages`
   * Spring Boot based kie-server: `/rest/server/containers/instances/mortgages`

using this payload
```json
{
    "lookup": null,
    "commands": [
      {
        "insert": {
          "object": {
            "mortgages.mortgages.Applicant": {
              "name": "Rafael",
              "age": 37,
              "applicationDate": "2021-05-14T16:48:00.000Z",
              "creditRating": "OK"
            }
          },
          "out-identifier": "Applicant",
          "return-object": true
        }
      },
      {
        "insert": {
          "object": {
            "mortgages.mortgages.LoanApplication": {
              "amount": 500000,
              "lengthYears": 15
            }
          },
          "out-identifier": "LoanApplication",
          "return-object": true
        }
      },
      {
        "insert": {
          "object": {
            "mortgages.mortgages.IncomeSource": {
              "amount": 160000,
              "type": "Job"
            }
          },
          "out-identifier": "IncomeSource",
          "return-object": true
        }
      },
      {
        "insert": {
          "object": {
            "mortgages.mortgages.Bankruptcy": {
              "amountOwed": 10001,
              "yearOfOccurrence": 2001
            }
          },
          "out-identifier": "Bankruptcy",
          "return-object": true
        }
      },
      {
        "fire-all-rules": {
          "max": -1,
          "out-identifier": "fired rules"
        }
      }
    ]
  }
```

you should get a response like this:
```json
{
  "type": "SUCCESS",
  "msg": "Container mortgages successfully called.",
  "result": {
    "execution-results": {
      "results": [
        {
          "value": 3,
          "key": "fired rules"
        },
        {
          "value": {
            "mortgages.mortgages.LoanApplication": {
              "amount": 500000,
              "approved": false,
              "approvedRate": null,
              "deposit": null,
              "explanation": "Only AA",
              "insuranceCost": null,
              "lengthYears": 15
            }
          },
          "key": "LoanApplication"
        },
        {
          "value": {
            "mortgages.mortgages.Applicant": {
              "age": 37,
              "applicationDate": 1621010880000,
              "approved": true,
              "creditRating": "OK",
              "name": "Rafael"
            }
          },
          "key": "Applicant"
        },
        {
          "value": {
            "mortgages.mortgages.IncomeSource": {
              "amount": 160000,
              "type": "Job"
            }
          },
          "key": "IncomeSource"
        },
        {
          "value": {
            "mortgages.mortgages.Bankruptcy": {
              "amountOwed": 10001,
              "yearOfOccurrence": 2001
            }
          },
          "key": "Bankruptcy"
        }
      ],
      "facts": [
        {
          "value": {
            "org.drools.core.common.DefaultFactHandle": {
              "external-form": "0:6:1978272422:1978272422:6:DEFAULT:NON_TRAIT:mortgages.mortgages.LoanApplication"
            }
          },
          "key": "LoanApplication"
        },
        {
          "value": {
            "org.drools.core.common.DefaultFactHandle": {
              "external-form": "0:5:1354478741:1354478741:5:DEFAULT:NON_TRAIT:mortgages.mortgages.Applicant"
            }
          },
          "key": "Applicant"
        },
        {
          "value": {
            "org.drools.core.common.DefaultFactHandle": {
              "external-form": "0:7:70623761:70623761:7:DEFAULT:NON_TRAIT:mortgages.mortgages.IncomeSource"
            }
          },
          "key": "IncomeSource"
        },
        {
          "value": {
            "org.drools.core.common.DefaultFactHandle": {
              "external-form": "0:8:1499955234:1499955234:8:DEFAULT:NON_TRAIT:mortgages.mortgages.Bankruptcy"
            }
          },
          "key": "Bankruptcy"
        }
      ]
    }
  }
}
```