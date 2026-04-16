Fixes #581

## Type of Change
- [x] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] This change requires a documentation update

## What changed and why
The `EVIDENCE_MAPPERS` dict in `post_process.py` was missing mapping functions for several EKS tools. Because of this gap, even when the agent successfully executed tools like `list_eks_pods` or `get_eks_events`, the data was silently dropped and never made it into the investigation state. 

This PR:
1. Adds the missing EKS mapping functions (`_map_eks_pods`, `_map_eks_events`, `_map_eks_deployments`, `_map_eks_node_health`, `_map_eks_pod_logs`).
2. Registers them into `EVIDENCE_MAPPERS`.
3. Updates `build_evidence_summary()` so the EKS tracker appropriately reports the data in the summary logs.

## Testing steps with evidence
- Added parameterized unit tests in `tests/nodes/investigate/test_post_process.py`.
- The tests verify that executing each EKS action correctly merges the expected keys (`eks_pods`, `eks_events`, etc.) into the main evidence dictionary.

## Impact analysis
- **Backward Compatibility:** Yes, fully compatible.
- **Breaking Changes:** None. This strictly appends newly fetched data safely.

## AI-Assisted Contribution
- [x] I have reviewed every line of code.
- [x] I understand the logic.
- [x] I have tested edge cases.
- [x] I have verified the code matches the project conventions.