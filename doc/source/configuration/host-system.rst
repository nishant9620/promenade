HostSystem
==========

Sample Document
---------------

.. code-block:: yaml

    schema: promenade/HostSystem/v1
    metadata:
      schema: metadata/Document/v1
      name: host-system
      layeringDefinition:
        abstract: false
        layer: site
    data:
      files:
        - path: /opt/kubernetes/bin/kubelet
          tar_url: https://dl.k8s.io/v1.11.6/kubernetes-node-linux-amd64.tar.gz
          tar_path: kubernetes/node/bin/kubelet
          mode: 0555
      images:
        haproxy: haproxy:1.8.3
        helm:
          helm: lachlanevenson/k8s-helm:v2.14.0
        kubernetes:
          kubectl: gcr.io/google_containers/hyperkube-amd64:v1.11.6
      packages:
        repositories:
          - deb http://apt.dockerproject.org/repo ubuntu-xenial main
        keys:
          - |-
            -----BEGIN PGP PUBLIC KEY BLOCK-----

            mQINBFWln24BEADrBl5p99uKh8+rpvqJ48u4eTtjeXAWbslJotmC/CakbNSqOb9o
            ddfzRvGVeJVERt/Q/mlvEqgnyTQy+e6oEYN2Y2kqXceUhXagThnqCoxcEJ3+KM4R
            mYdoe/BJ/J/6rHOjq7Omk24z2qB3RU1uAv57iY5VGw5p45uZB4C4pNNsBJXoCvPn
            TGAs/7IrekFZDDgVraPx/hdiwopQ8NltSfZCyu/jPpWFK28TR8yfVlzYFwibj5WK
            dHM7ZTqlA1tHIG+agyPf3Rae0jPMsHR6q+arXVwMccyOi+ULU0z8mHUJ3iEMIrpT
            X+80KaN/ZjibfsBOCjcfiJSB/acn4nxQQgNZigna32velafhQivsNREFeJpzENiG
            HOoyC6qVeOgKrRiKxzymj0FIMLru/iFF5pSWcBQB7PYlt8J0G80lAcPr6VCiN+4c
            NKv03SdvA69dCOj79PuO9IIvQsJXsSq96HB+TeEmmL+xSdpGtGdCJHHM1fDeCqkZ
            hT+RtBGQL2SEdWjxbF43oQopocT8cHvyX6Zaltn0svoGs+wX3Z/H6/8P5anog43U
            65c0A+64Jj00rNDr8j31izhtQMRo892kGeQAaaxg4Pz6HnS7hRC+cOMHUU4HA7iM
            zHrouAdYeTZeZEQOA7SxtCME9ZnGwe2grxPXh/U/80WJGkzLFNcTKdv+rwARAQAB
            tDdEb2NrZXIgUmVsZWFzZSBUb29sIChyZWxlYXNlZG9ja2VyKSA8ZG9ja2VyQGRv
            Y2tlci5jb20+iQI4BBMBAgAiBQJVpZ9uAhsvBgsJCAcDAgYVCAIJCgsEFgIDAQIe
            AQIXgAAKCRD3YiFXLFJgnbRfEAC9Uai7Rv20QIDlDogRzd+Vebg4ahyoUdj0CH+n
            Ak40RIoq6G26u1e+sdgjpCa8jF6vrx+smpgd1HeJdmpahUX0XN3X9f9qU9oj9A4I
            1WDalRWJh+tP5WNv2ySy6AwcP9QnjuBMRTnTK27pk1sEMg9oJHK5p+ts8hlSC4Sl
            uyMKH5NMVy9c+A9yqq9NF6M6d6/ehKfBFFLG9BX+XLBATvf1ZemGVHQusCQebTGv
            0C0V9yqtdPdRWVIEhHxyNHATaVYOafTj/EF0lDxLl6zDT6trRV5n9F1VCEh4Aal8
            L5MxVPcIZVO7NHT2EkQgn8CvWjV3oKl2GopZF8V4XdJRl90U/WDv/6cmfI08GkzD
            YBHhS8ULWRFwGKobsSTyIvnbk4NtKdnTGyTJCQ8+6i52s+C54PiNgfj2ieNn6oOR
            7d+bNCcG1CdOYY+ZXVOcsjl73UYvtJrO0Rl/NpYERkZ5d/tzw4jZ6FCXgggA/Zxc
            jk6Y1ZvIm8Mt8wLRFH9Nww+FVsCtaCXJLP8DlJLASMD9rl5QS9Ku3u7ZNrr5HWXP
            HXITX660jglyshch6CWeiUATqjIAzkEQom/kEnOrvJAtkypRJ59vYQOedZ1sFVEL
            MXg2UCkD/FwojfnVtjzYaTCeGwFQeqzHmM241iuOmBYPeyTY5veF49aBJA1gEJOQ
            TvBR8Q==
            =Fm3p
            -----END PGP PUBLIC KEY BLOCK-----
        additional:
          - curl
          - jq
        required:
          docker: docker-engine=1.13.1-0~ubuntu-xenial
          socat: socat=1.7.3.1-1


Files
-----

A list of files to be written to the host.  Files can be given as precise content or extracted from a tarball specified by url:

.. code-block:: yaml

    - path: /etc/direct-content
      content: |-
        This
        exact
        text
    - path: /etc/from-tar
      tar_url: http://example.com/file
      tar_source: dir/file.txt

Images
------

Core Images
^^^^^^^^^^^

These images are used for essential functionality:

``haproxy``
    HAProxy_ is configured and used for Kubernetes API discovery during
    bootstrapping.

``kubectl``
    Used for label application and validation tasks during bootstrapping.

.. _HAProxy: https://www.haproxy.org/


Convenience Images
^^^^^^^^^^^^^^^^^^

The ``helm`` image is available for convenience.


Packages
--------

Repository Configuration
^^^^^^^^^^^^^^^^^^^^^^^^

Additional APT repositories can be configured using the ``repositories`` and
``keys`` fields of the ``SystemPackages`` document:

``repositories``
    A list of APT source lines to be configured during genesis or join.

``keys``
    A list of public PGP keys that can be used to verify installed packages.


Package Configuration
^^^^^^^^^^^^^^^^^^^^^

The ``required`` key specifies packages that are required for all deployments,
and the ``additional`` key allows arbitrary additional system packages to be
installed.  The ``additional`` key is particularly useful for installing
packages such as `ceph-common`.
