## Subclass Example

Lets say we have a class that we would like to work with on
the command line, but want to keep the class itself unchanaged
without mixin.

    class Hello
      attr_accessor :name

      def initialize(name="World")
        @name = name
      end

      def hello
        @output = "Hello, #{name}!"
      end

      def output
        @output
      end
    end

Rather then including Exectuable in the class directly, we can
create a subclass and use it instead.

    class HelloCommand < Hello
      include Executable

      def call(*args)
        hello
      end
    end

Now we can execute the command perfectly well.

    cmd = HelloCommand.execute(['hello', '--name=Fred'])
    cmd.output.assert == "Hello, Fred!"

And the original class remains undisturbed.

