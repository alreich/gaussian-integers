gint: Gaussian Integers/Rationals + Gaussian-Integer RSA
=========================================================

**gint** provides two related numeric types:

- :class:`~gint.zi.Zi` -- Gaussian integers, :math:`a + bi` with :math:`a, b \in \mathbb{Z}`.
- :class:`~gint.qi.Qi` -- Gaussian rationals, :math:`a + bi` with :math:`a, b \in \mathbb{Q}`,
  represented exactly via :class:`fractions.Fraction`.

Also provides **Gaussian-integer RSA cryptography** (teaching implementation only; not for real secrets)

Note: A ``Qi`` whose components both reduce to whole numbers is automatically
returned as a ``Zi`` instead -- ``Qi(4, 6)`` *is* a ``Zi(4, 6)``.

Quickstart
----------

Install from GitHub, then import both classes. Both are needed because
division of ``Zi`` values can produce a ``Qi``, and a ``Qi`` with whole-number
parts collapses back into a ``Zi``.

.. code-block:: console

   $ pip install git+https://github.com/alreich/gaussian-integers.git

.. code-block:: pycon

   >>> from gint import Zi, Qi

Creating values
~~~~~~~~~~~~~~~

.. code-block:: pycon

   >>> z1, z2, z3 = Zi(2, -3), Zi(1, 4), Zi(-8, 1)
   >>> z1
   Zi(2, -3)
   >>> print(z1)
   (2-3j)
   >>> Qi(1.25, 3.4)
   Qi('5/4', '17/5')
   >>> print(Zi(5, 0))
   5

Arithmetic
~~~~~~~~~~

Dividing Gaussian integers gives a ``Zi`` when the division is exact and a
``Qi`` otherwise.

.. code-block:: pycon

   >>> z12 = z1 * z2
   >>> z12
   Zi(14, 5)
   >>> z12 / z1
   Zi(1, 4)
   >>> z12 / z3
   Qi('-107/65', '-54/65')
   >>> (z12 / z3) * z3
   Zi(14, 5)
   >>> 1 / Zi(1, 1)
   Qi('1/2', '-1/2')

Number theory
~~~~~~~~~~~~~

Results are determined only up to multiplication by a unit
(:math:`\pm 1, \pm i`).

.. code-block:: pycon

   >>> Zi.gcd(z12, z1)
   Zi(2, -3)
   >>> Zi.lcm(z12, z1)
   Zi(14, 5)
   >>> Zi.is_gaussian_prime(Zi(3, 0))
   True

Gaussian RSA
~~~~~~~~~~~~

.. code-block:: pycon

   >>> from gint.crypto import generate_keypair, encrypt_text, decrypt_text
   >>> public_key, private_key = generate_keypair(bits=256)
   >>> ciphertext = encrypt_text("Gaussian primes are cool.", public_key)
   >>> decrypt_text(ciphertext, private_key)
   'Gaussian primes are cool.'

.. toctree::
   :maxdepth: 2
   :caption: API Reference

   zi
   qi
   crypto
