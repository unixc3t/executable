## README Example

This is the example used in the documentation.

    class Example
      include Executable

      attr_switch :quiet

      def bread(*args)
        ["bread", quiet?, *args]
      end

      def butter(*args)
        ["butter", quiet?, *args]
      end

      # Route call to methods.
      def call(name, *args)
        meth = public_method(name)
        meth.call(*args)
      end
    end

Use a subcommand and an argument.

    c, a = Example.parse(['butter', 'yum'])
    r = c.call(*a)
    r.assert == ["butter", nil, "yum"]

A subcommand and a boolean option.

    c, a = Example.parse(['bread', '--quiet'])
    r = c.call(*a)
    r.assert == ["bread", true]

