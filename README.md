# grnsnake
Snake with a single variable

# how
i used offsets into a 1024kb memory area as variables using macros, then i substituted macro calls. check `readable.c`.
could use less offsets if we made everything live in the SDL_Event's padding.
