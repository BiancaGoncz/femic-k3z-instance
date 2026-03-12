Rebuild and QA
==============

Deterministic Rebuild Script
----------------------------

Use instance-local FEMIC commands for reproducible K3Z rebuild checks:

.. code-block:: bash

   femic run --run-config config/run_profile.k3z.yaml --run-id k3z_rebuild_check
   femic patchworks build-blocks --config config/patchworks.runtime.windows.yaml
   femic patchworks matrix-build --config config/patchworks.runtime.windows.yaml --run-id k3z_rebuild_check

Outputs
-------

- Rebuild report:
  instance workflow report artifact (if configured for your deployment)
- Matrix logs:
  ``vdyp_io/logs/patchworks_matrixbuilder_{stdout,stderr,manifest}-<run_id>.log``

Key Invariants
--------------

- Managed area should remain near expected baseline.
- Block joins must remain 1:1 between ``tracks/blocks.csv`` and ``blocks/blocks.shp``.
- Seral accounts must exist in ``tracks/accounts.csv``.
- Required managed species yields must remain non-zero (except explicitly allowed species).

Baseline Workflow
-----------------

Initialize accepted baseline evidence (once per accepted model state):

.. code-block:: bash

   femic run --run-config config/run_profile.k3z.yaml --run-id k3z_baseline
   femic patchworks matrix-build --config config/patchworks.runtime.windows.yaml --run-id k3z_baseline

Validate against baseline:

.. code-block:: bash

   femic run --run-config config/run_profile.k3z.yaml --run-id k3z_compare
   femic patchworks matrix-build --config config/patchworks.runtime.windows.yaml --run-id k3z_compare
