## Summary

Describe the requirement and the implemented change.

## Verification

- [ ] Relevant focused tests pass.
- [ ] `verifyLostHubBe` passes.
- [ ] New or changed behavior has appropriate test coverage.
- [ ] Changed public or protected APIs have accurate Korean Javadoc.

## Architecture And Safety

- [ ] The HTTP flow respects `Router -> Controller -> Service -> Repository`.
- [ ] WebSocket handlers delegate business behavior to services.
- [ ] Authentication, authorization, transaction, and concurrency rules remain valid.
- [ ] No secret or production credential is included.
- [ ] No Docker-related configuration is introduced.

## External Contract And Deployment Impact

- External API contract changed: `yes / no`
- Contract change summary:
- Railway deployment impact:
