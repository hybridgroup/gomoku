# Gomoku

Gomoku is a compiler for programs written in the Go Programming
Language, targeting modern C++.

This is an experiment to determine how well Go will perform on embedded
devices, where a port of existing compilers would prove to be actually
more complicated due to the different expectations from platforms
supported by gc and devices with very few kilobytes of RAM.

The bulk of the compiler has been written in a day (April 1st, 2017),
but in reality, it's the author's second attempt at doing so.  The
lexer, parser, and type system are part of the standard library
already, so it was just the matter of tying everything together.

The generated code still doesn't compile: only the input program itself
is generated.  All the type information and constants from imported
packages is also generated, but no functions or methods from those are
actually generated.

Also, the code has been hacked together in a little bit less than a day
worth of work, so there are many mistakes and opportunities to
refactor.  Of note, no testing is performed.  Any help to move this
forward is greatly appreciated.

# Sample code

## surface (from chapter 3 of "The Go Programming Language" by Kernigham and Donovan)

[Full code here](https://gist.github.com/lpereira/8bc64bf9796984b7868b8255d1692d59), excerpt below:

### Go

    func corner(i, j int) (float64, float64) {
            // Find point (x,y) at corner of cell (i,j).
            x := xyrange * (float64(i)/cells - 0.5)
            y := xyrange * (float64(j)/cells - 0.5)

            // Compute surface height z.
            z := f(x, y)

            // Project (x,y,z) isometrically onto 2-D SVG canvas (sx,sy).
            sx := width/2 + (x-y)*cos30*xyscale
            sy := height/2 + (x+y)*sin30*xyscale - z*zscale
            return sx, sy
    }


### C++

    std::pair<double, double> corner(int i, int j) {
      double sx{0};
      double sy{0};
      double x{0};
      double y{0};
      double z{0};
      x = xyrange * (float64(i) / cells - 0.5);
      y = xyrange * (float64(j) / cells - 0.5);
      z = f(x, y);
      sx = width / 2 + (x - y) * cos30 * xyscale;
      sy = height / 2 + (x + y) * sin30 * xyscale - z * zscale;
      return std::pair<double, double>(sx, sy);
    }
