## Multiple Subcommmands

Setup an example CLI subclass.

    class MyCLI < Executable::Command
      attr :result

      def initialize
        @result = []
      end

      def g=(value)
        @result << "g" if value
      end

      def g?
        @result.include?("g")
      end

      #
      class C1 < self
        def call
          @result << "c1"
        end

        def o1=(value)
          @result << "c1_o1 #{value}"
        end

        def o2=(value)
          @result << "c1_o2 #{value}"
        end
      end

      #
      class C2 < Executable::Command
        attr :result

        def initialize
          @result = []
        end

        def call
          @result << "c2"
        end

        def o1=(value)
          @result << "c2_o1 #{value}"
        end

        def o2=(value)
          @result << "c2_o2" if value
        end

        def o2?
          @result.include?("c2_o2")
        end
      end

    end

Instantiate and run the class on an example command line.

Just a command.

    cli = MyCLI.run('c1')
    cli.result.assert == ['c1']

Command with global option.

    cli = MyCLI.run('c1 -g')
    cli.result.assert == ['g', 'c1']

Command with an option.

    cli = MyCLI.run('c1 --o1 A')
    cli.result.assert == ['c1_o1 A', 'c1']

Command with two options.

    cli = MyCLI.run('c1 --o1 A --o2 B')
    cli.result.assert == ['c1_o1 A', 'c1_o2 B', 'c1']

Try out the second command.

    cli = MyCLI.run('c2')
    cli.result.assert == ['c2']

Seoncd command with an option.

    cli = MyCLI.run('c2 --o1 A')
    cli.result.assert == ['c2_o1 A', 'c2']

Second command with two options.

    cli = MyCLI.run('c2 --o1 A --o2')
    cli.result.assert == ['c2_o1 A', 'c2_o2', 'c2']

Since C1#main takes not arguments, if we try to issue a command
that will have left over arguments, then an ArgumentError will be raised.

    expect ArgumentError do
      cli = MyCLI.run('c1 a')
    end

How about a non-existenct subcommand.

    expect NotImplementedError do
      cli = MyCLI.run('q')
      cli.result.assert == ['q']
    end

How about an option only.

    expect NotImplementedError do
      cli = MyCLI.run('-g')
      cli.result.assert == ['-g']
    end

How about a non-existant options.

    expect Executable::NoOptionError do
      MyCLI.run('c1 --foo')
    end

