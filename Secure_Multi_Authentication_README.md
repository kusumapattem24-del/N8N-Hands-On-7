# Secure Multi-Authentication Workflow - n8n

**Assessment:** Build & Validate a Secure Multi-Authentication Workflow  
**Status:** Documentation template / implementation plan; actual execution evidence must be added after testing.

## 1. Objective
Build three independent authentication branches in n8n using API Key, Bearer Token, and OAuth2. Store secrets only in n8n Credential Manager, verify request authorization, and document test outcomes and security controls.

## 2. Workflow architecture
```text
Manual Trigger
  |-- HTTP_APIKey_Request (Header Auth credential) --> APIKey_Validate_Response
  |-- HTTP_Bearer_Request (Bearer Auth credential) --> Bearer_Validate_Response
  `-- HTTP_OAuth2_Request (OAuth2 credential) --> OAuth2_Validate_Response
```
Each HTTP Request connects directly to the Manual Trigger. Add Sticky Notes documenting the credential type, expected response, and test limitations. Branches are logically independent; actual scheduling/execution depends on n8n workflow behavior.

## 3. Configuration guide
### API Key branch
1. Add HTTP Request node `HTTP_APIKey_Request` and choose GET `https://postman-echo.com/headers`.
2. Under Authentication choose Generic Credential Type > Header Auth (wording varies by n8n version).
3. Create a Header Auth credential with name `X-API-Key` and a **test-only** key value stored in Credential Manager.
4. Execute; inspect the echoed header name. **An echo endpoint confirms header transmission, not whether the key is accepted by a protected service.**

### Bearer Token branch
1. Add HTTP Request node `HTTP_Bearer_Request`, GET `https://postman-echo.com/headers`.
2. Choose Bearer Auth credential (or supported generic header credential) and store a **test-only** token securely.
3. Confirm the Authorization header is sent. Do not include raw tokens in screenshots or public artifacts. Header echo does not establish token validity.

### OAuth2 branch
1. Select a provider with a protected test resource and register an OAuth client.
2. Set the redirect URI to the **exact OAuth callback URL shown in your n8n credential UI**. Configure authorization URL, token URL, client ID, client secret, required scopes and grant type according to provider documentation.
3. Store the OAuth2 client secret only in n8n Credential Manager; complete Connect/Sign in.
4. Configure `HTTP_OAuth2_Request` to access a provider-specific protected endpoint using the OAuth2 credential.
5. Verify HTTP success and an expected authorized resource response; capture **redacted** evidence. Record provider, scope and endpoint used in the evidence log.

## 4. Authentication comparison
| Method | Credential | Typical use | Verification |
|---|---|---|---|
| API Key | Header Auth / API key credential | Service integrations | Header present + protected API response |
| Bearer Token | Bearer Auth credential | Token-based APIs | Authorization header + protected API response |
| OAuth2 | OAuth2 credential | Delegated, scoped API access | Successful authorization + protected resource |

## 5. Security review
- Do not hardcode keys, tokens, client secrets or passwords in nodes, expressions, exports or GitHub.
- Apply least-privilege OAuth scopes and dedicated integration credentials; rotate and revoke exposed secrets.
- Limit access to n8n credentials and execution data; redact authorization headers and customer data in screenshots.
- Prefer HTTPS endpoints, use provider-approved OAuth redirect URIs, and avoid putting secrets in query strings.
- Review the exported workflow JSON and screenshots for secrets before public upload. **Do not publish credential exports.**

## 6. Troubleshooting
| Failure | Possible cause | Remediation |
|---|---|---|
| 401 Unauthorized | Wrong/expired key or token | Confirm credential, token audience/expiry, and service access |
| Missing authorization header | Wrong auth mode / credential not selected | Re-select credential and inspect request configuration |
| OAuth invalid redirect URI | Callback mismatch | Match exact n8n callback URI in provider app registration |
| Credential unavailable | Permission or credential not shared | Verify access rights and credential selection |
| 403 Forbidden | Insufficient scopes/permissions | Request minimum necessary permissions and re-authorize |
| Secret leaked in output | Echo endpoint or execution logs exposed value | Redact logs, remove artifact, rotate compromised secret |

## 7. Test matrix (record real results)
| ID | Test | Expected result | Actual result | Evidence |
|---|---|---|---|---|
| AUTH-01 | API key request with configured credential | Header injected; echo returns header | Not tested | Pending screenshot |
| AUTH-02 | Bearer token request with configured credential | Authorization header injected | Not tested | Pending screenshot |
| AUTH-03 | OAuth2 authorization | Credential connects successfully | Not tested | Pending screenshot |
| AUTH-04 | OAuth2 protected resource request | 2xx + expected resource data | Not tested | Pending screenshot |
| AUTH-05 | Invalid API key against protected API | 401/403 as documented by provider | Not tested | Pending screenshot |
| AUTH-06 | Expired/invalid bearer token against protected API | Unauthorized response | Not tested | Pending screenshot |
| AUTH-07 | OAuth redirect URI mismatch | Provider rejects authorization | Not tested | Pending screenshot |
| AUTH-08 | Secret review of public export | No secret values exposed | Not tested | Pending review |

## 8. Evidence capture plan
- `screenshots/01-workflow-overview.png`: Manual Trigger and three branches.
- `screenshots/02-api-key-config-redacted.png`: Credential type and API Key HTTP Request settings.
- `screenshots/03-api-key-response-redacted.png`: API key header validation.
- `screenshots/04-bearer-config-redacted.png`: Bearer credential and request settings.
- `screenshots/05-bearer-response-redacted.png`: Bearer header validation.
- `screenshots/06-oauth-connected-redacted.png`: OAuth2 connection status without secrets.
- `screenshots/07-oauth-resource-redacted.png`: Successful protected API response.
- `screenshots/08-execution-overview.png`: Successful execution path.

**Evidence status:** No screenshots or live execution results were provided with this request; the entries above are placeholders, not proof of completion.

## 9. Suggested repository structure
```text
secure-multi-authentication/
  README.md
  workflow/multi-auth-workflow.json
  docs/Secure_Multi_Authentication_Documentation.pdf
  screenshots/ (redacted evidence)
  tests/authentication-test-results.md
```

## 10. Submission checklist
- [ ] Three authentication branches implemented and executed
- [ ] Credentials managed in n8n; no secrets in source or screenshots
- [ ] OAuth2 connection and protected-resource response verified
- [ ] Sticky Notes and descriptive node names included
- [ ] Test matrix filled with observed outcomes
- [ ] Workflow JSON exported and inspected for secrets
- [ ] Public GitHub repository created and verified
- [ ] Demonstration video uploaded and link tested
- [ ] PDF documentation and redacted screenshots included

## 11. Demo video outline (5-8 minutes)
1. Introduce architecture and three branches.
2. Show API Key credential type, HTTP request and redacted response.
3. Show Bearer credential type, HTTP request and redacted response.
4. Show OAuth2 connection and authorized protected resource.
5. Walk through security review, troubleshooting, and test matrix.
6. Show exported JSON, public repository, and required documentation.

## 12. LMS description (edit after execution)
Implemented a three-branch n8n authentication design covering API Key, Bearer Token and OAuth2. Credentials are intended to be managed securely through n8n Credential Manager. Add verified execution results, actual challenges/resolutions, OAuth provider/scopes, limitations, and links before submission.

**Repository:** [ADD PUBLIC GITHUB URL]  
**Video:** [ADD LOOM/YOUTUBE URL]  
**OAuth provider:** [ADD PROVIDER]  
**Known limitations:** [ADD VERIFIED DETAILS]
