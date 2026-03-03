# grnsnake
Snake with a single variable

- [how](#how)
- [code](#code)
- [readable code](#readable-code)

# how
i started using variables, then converted each one into offsets into a 1024kb memory area as variables using macros, then i inlined some, 
then i substituted macro calls. check [readable code](#readable-code) for the pre-substitution code. could use less offsets if we made everything live in the SDL_Event's padding.

# code
```c
#include <stdio.h>
#include <stdlib.h>
#include <SDL3/SDL.h>

#define SCREEN_WIDTH 800
#define SCREEN_HEIGHT 600
#define GRID_X 16
#define GRID_Y 16

typedef struct {
    int isSnake;
    int isWall;
    int isFood;
} cell_t;

uint8_t *g; // allahvariable

void InitGrid() {
    srand(SDL_GetTicks());

    for (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) = 0;
         *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) < GRID_X * GRID_Y;
         (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                     sizeof(cell_t) * GRID_X * GRID_Y)))++) {
        memset(
            &((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(
                              int)))[*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) +
                                                sizeof(int) + sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))], 0,
            sizeof(cell_t));
    }

    for (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) = 0;
         *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) < GRID_X;
         (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                     sizeof(cell_t) * GRID_X * GRID_Y)))++) {
        ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
        [*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof
                    (int) + sizeof(cell_t) * GRID_X * GRID_Y))].isWall = 1;
        ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
        [(*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                     sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))) + GRID_X * (GRID_Y - 1)].isWall = 1;
    }
    for (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) = 0;
         *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) < GRID_Y; (*((int *) (
             g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
                 cell_t) * GRID_X * GRID_Y)))++) {
        ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
        [*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof
                    (int) + sizeof(cell_t) * GRID_X * GRID_Y)) * GRID_X].isWall = 1;
        ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
        [*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof
                    (int) + sizeof(cell_t) * GRID_X * GRID_Y)) * GRID_X + (GRID_X - 1)].isWall = 1;
    }

    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
               sizeof(cell_t) * GRID_X * GRID_Y)) = (GRID_Y / 2) * GRID_X + (GRID_X / 2);
    ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))[*((
        int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y))].isSnake = 1;
    ((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof
              (cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))[0] = *((int *) (
        g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
            cell_t) * GRID_X * GRID_Y));
    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
               sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2)) = 1;

    for (;;) {
        *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y)) = rand() % (GRID_X * GRID_Y);
        if (!((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(
                              int)))[*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) +
                                                sizeof(int) + sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))].isWall
            && !((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                             sizeof(int)))[*((int *) (
                g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y))].isSnake) {
            ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(
                             int)))[*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) +
                                               sizeof(int) + sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))].isFood =
                    1;
            break;
        }
    }
}

void DrawGrid() {
    SDL_SetRenderDrawColor(*((SDL_Renderer **) (g + sizeof(SDL_Window *))), 255, 255, 255, 255);
    SDL_RenderClear(*((SDL_Renderer **) (g + sizeof(SDL_Window *))));

    for (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) = 0;
         *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y)) < GRID_X * GRID_Y; (*((int *) (
             g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
                 cell_t) * GRID_X * GRID_Y)))++) {
        ((SDL_FRect *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))))->x =
        ((*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                     sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))) % GRID_X) * (SCREEN_WIDTH / GRID_X);
        ((SDL_FRect *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))))->y =
        ((*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                     sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))) / GRID_X) * (SCREEN_HEIGHT / GRID_Y);
        ((SDL_FRect *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))))->w = SCREEN_WIDTH / GRID_X;
        ((SDL_FRect *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))))->h = SCREEN_HEIGHT / GRID_Y;

        if (((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(
                             int)))[*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) +
                                               sizeof(int) + sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))].isWall)
            SDL_SetRenderDrawColor(*((SDL_Renderer **) (g + sizeof(SDL_Window *))), 128, 128, 128, 255);
        else if (((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                              sizeof(int)))[*((int *) (
            g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
                cell_t) * GRID_X * GRID_Y))].isSnake)
            SDL_SetRenderDrawColor(*((SDL_Renderer **) (g + sizeof(SDL_Window *))), 0, 0, 0, 255);
        else if (((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                              sizeof(int)))[*((int *) (
            g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
                cell_t) * GRID_X * GRID_Y))].isFood)
            SDL_SetRenderDrawColor(*((SDL_Renderer **) (g + sizeof(SDL_Window *))), 255, 0, 0, 255);
        else
            continue;

        SDL_RenderFillRect(*((SDL_Renderer **) (g + sizeof(SDL_Window *))),
                           (SDL_FRect *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))));
    }

    SDL_RenderPresent(*((SDL_Renderer **) (g + sizeof(SDL_Window *))));
}

void MoveSnake() {
    *(int *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))) =
    ((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)
              + sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))[
        *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2)) - 1] % GRID_X;
    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
               sizeof(cell_t) * GRID_X * GRID_Y)) =
    ((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof
              (cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))[
        *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2)) - 1] / GRID_X;
    *(int *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))) += *((int *) (
        g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event)));
    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
               sizeof(cell_t) * GRID_X * GRID_Y)) += *((int *) (
        g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int)));

    if (*(int *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))) < 0)
        *(int *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))) = 0;
    if (*(int *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))) >= GRID_X)
        *(int *) ((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))) = GRID_X - 1;
    if (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y)) < 0)
        *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y)) = 0;
    if (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y)) >= GRID_Y)
        *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y)) = GRID_Y - 1;

    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
               sizeof(cell_t) * GRID_X * GRID_Y)) =
            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)
                       +
                       sizeof(cell_t) * GRID_X * GRID_Y)) * GRID_X + *(int *) ((SDL_Event *) (
                g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *)));

    if (((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
        [*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y))].isWall || ((cell_t *) (
            g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))[*((int
            *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y))].isSnake) {
        printf("you lost at grnsnake with allahvariable :(\n");
        exit(0);
    }

    ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))[*((
        int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y))].isSnake = 1;
    ((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof
              (cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))[*((int *) (
        g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
            cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2))] = *((int *) (
        g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
            cell_t) * GRID_X * GRID_Y));
    (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2)))++;

    if (((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
    [*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y))].isFood) {
        ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
        [*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof
                    (int) + sizeof(cell_t) * GRID_X * GRID_Y))].isFood = 0;

        for (;;) {
            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)
                       + sizeof(cell_t) * GRID_X * GRID_Y)) = rand() % (GRID_X * GRID_Y);
            if (!((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                              sizeof(int)))[*((int *) (
                    g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y))].isWall && !((cell_t *) (
                    g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))[
                    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                               sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))].isSnake) {
                ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) +
                             sizeof(int)))[*((int *) (
                    g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y))].isFood = 1;
                break;
            }
        }
    } else {
        *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                   sizeof(cell_t) * GRID_X * GRID_Y)) = ((int *) (
            g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(
                cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))[0];
        ((cell_t *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
        [*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof
                    (int) + sizeof(cell_t) * GRID_X * GRID_Y))].isSnake = 0;

        for (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(
                            int) + sizeof(cell_t) * GRID_X * GRID_Y)) = 1;
             *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(
                            int) + sizeof(cell_t) * GRID_X * GRID_Y)) < *((int *) (
                 g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                 sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2)); (*((int *) (
                 g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                 sizeof(cell_t) * GRID_X * GRID_Y)))++) {
            ((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)
                      + sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))[
                *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(
                               int) + sizeof(cell_t) * GRID_X * GRID_Y)) - 1] = ((int *) (
                g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))[*((int *) (
                g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                sizeof(cell_t) * GRID_X * GRID_Y))];
        }
        (*((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) +
                    sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2)))--;
    }
}

int main(void) {
    SDL_Init(SDL_INIT_VIDEO | SDL_INIT_EVENTS);
    g = (uint8_t *) malloc(
        1024 * 1024
    );

    InitGrid();
    SDL_CreateWindowAndRenderer("grnsnake", SCREEN_WIDTH, SCREEN_HEIGHT, 0, ((SDL_Window **) (g)),
                                ((SDL_Renderer **) (g + sizeof(SDL_Window *))));

    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event))) = 0;
    *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(int))) = -1;

    while (1) {
        while (SDL_PollEvent(((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *))))) {
            switch (((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *)))->type) {
                case SDL_EVENT_QUIT:
                    return 1;
                case SDL_EVENT_KEY_DOWN:
                    switch (((SDL_Event *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *)))->key.scancode) {
                        case SDL_SCANCODE_W:
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event))) = 0;
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(
                                           int))) = -1;
                            break;
                        case SDL_SCANCODE_S:
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event))) = 0;
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(
                                           int))) = 1;
                            break;
                        case SDL_SCANCODE_A:
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event))) = -1;
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(
                                           int))) = 0;
                            break;
                        case SDL_SCANCODE_D:
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event))) = 1;
                            *((int *) (g + sizeof(SDL_Window *) + sizeof(SDL_Renderer *) + sizeof(SDL_Event) + sizeof(
                                           int))) = 0;
                            break;
                    }
                default: break;
            }
        }

        MoveSnake();
        DrawGrid();
        SDL_Delay(100);
    }
}
```

# readable code
```c
#include <stdio.h>
#include <stdlib.h>
#include <SDL3/SDL.h>

#define SCREEN_WIDTH 800
#define SCREEN_HEIGHT 600
#define GRID_X 16
#define GRID_Y 16

typedef struct {
    int isSnake;
    int isWall;
    int isFood;
} cell_t;

uint8_t *g; // allahvariable

#define WINDOW_PTR ((SDL_Window**)(g))
#define RENDERER_PTR ((SDL_Renderer**)(g + sizeof(SDL_Window*)))
#define EVENT_PTR ((SDL_Event*)(g + sizeof(SDL_Window*) + sizeof(SDL_Renderer*)))
#define DX_PTR ((int*)(g + sizeof(SDL_Window*) + sizeof(SDL_Renderer*) + sizeof(SDL_Event)))
#define DY_PTR ((int*)(g + sizeof(SDL_Window*) + sizeof(SDL_Renderer*) + sizeof(SDL_Event) + sizeof(int)))
#define GRID_PTR ((cell_t*)(g + sizeof(SDL_Window*) + sizeof(SDL_Renderer*) + sizeof(SDL_Event) + sizeof(int) + sizeof(int)))
#define I_PTR ((int*)(g + sizeof(SDL_Window*) + sizeof(SDL_Renderer*) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y))
#define SNAKE_BODY_PTR ((int*)(g + sizeof(SDL_Window*) + sizeof(SDL_Renderer*) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y))
#define SNAKE_LENGTH_PTR ((int*)(g + sizeof(SDL_Window*) + sizeof(SDL_Renderer*) + sizeof(SDL_Event) + sizeof(int) + sizeof(int) + sizeof(cell_t) * GRID_X * GRID_Y + sizeof(int) * GRID_X * GRID_Y * 2))

void InitGrid() {
    srand(SDL_GetTicks());

    for (*I_PTR = 0; *I_PTR < GRID_X * GRID_Y; (*I_PTR)++) {
        memset(&GRID_PTR[*I_PTR], 0, sizeof(cell_t));
    }

    for (*I_PTR = 0; *I_PTR < GRID_X; (*I_PTR)++) {
        GRID_PTR[*I_PTR].isWall = 1;
        GRID_PTR[(*I_PTR) + GRID_X * (GRID_Y - 1)].isWall = 1;
    }
    for (*I_PTR = 0; *I_PTR < GRID_Y; (*I_PTR)++) {
        GRID_PTR[*I_PTR * GRID_X].isWall = 1;
        GRID_PTR[*I_PTR * GRID_X + (GRID_X - 1)].isWall = 1;
    }

    *I_PTR = (GRID_Y / 2) * GRID_X + (GRID_X / 2);
    GRID_PTR[*I_PTR].isSnake = 1;
    SNAKE_BODY_PTR[0] = *I_PTR;
    *SNAKE_LENGTH_PTR = 1;

    for (;;) {
        *I_PTR = rand() % (GRID_X * GRID_Y);
        if (!GRID_PTR[*I_PTR].isWall && !GRID_PTR[*I_PTR].isSnake) {
            GRID_PTR[*I_PTR].isFood = 1;
            break;
        }
    }
}

void DrawGrid() {
    SDL_SetRenderDrawColor(*RENDERER_PTR, 255, 255, 255, 255);
    SDL_RenderClear(*RENDERER_PTR);

    for (*I_PTR = 0; *I_PTR < GRID_X * GRID_Y; (*I_PTR)++) {
        ((SDL_FRect*)EVENT_PTR)->x = ((*I_PTR) % GRID_X) * (SCREEN_WIDTH / GRID_X);
        ((SDL_FRect*)EVENT_PTR)->y = ((*I_PTR) / GRID_X) * (SCREEN_HEIGHT / GRID_Y);
        ((SDL_FRect*)EVENT_PTR)->w = SCREEN_WIDTH / GRID_X;
        ((SDL_FRect*)EVENT_PTR)->h = SCREEN_HEIGHT / GRID_Y;

        if (GRID_PTR[*I_PTR].isWall)
            SDL_SetRenderDrawColor(*RENDERER_PTR, 128, 128, 128, 255);
        else if (GRID_PTR[*I_PTR].isSnake)
            SDL_SetRenderDrawColor(*RENDERER_PTR, 0, 0, 0, 255);
        else if (GRID_PTR[*I_PTR].isFood)
            SDL_SetRenderDrawColor(*RENDERER_PTR, 255, 0, 0, 255);
        else
            continue;

        SDL_RenderFillRect(*RENDERER_PTR, (SDL_FRect*)EVENT_PTR);
    }

    SDL_RenderPresent(*RENDERER_PTR);
}

void MoveSnake() {
    *(int*)EVENT_PTR = SNAKE_BODY_PTR[*SNAKE_LENGTH_PTR - 1] % GRID_X;
    *I_PTR = SNAKE_BODY_PTR[*SNAKE_LENGTH_PTR - 1] / GRID_X;
    *(int*)EVENT_PTR += *DX_PTR;
    *I_PTR += *DY_PTR;

    if (*(int*)EVENT_PTR < 0) *(int*)EVENT_PTR = 0;
    if (*(int*)EVENT_PTR >= GRID_X) *(int*)EVENT_PTR = GRID_X - 1;
    if (*I_PTR < 0) *I_PTR = 0;
    if (*I_PTR >= GRID_Y) *I_PTR = GRID_Y - 1;

    *I_PTR = *I_PTR * GRID_X + *(int*)EVENT_PTR;

    if (GRID_PTR[*I_PTR].isWall || GRID_PTR[*I_PTR].isSnake) {
        printf("Game Over!\n");
        exit(0);
    }

    GRID_PTR[*I_PTR].isSnake = 1;
    SNAKE_BODY_PTR[*SNAKE_LENGTH_PTR] = *I_PTR;
    (*SNAKE_LENGTH_PTR)++;

    if (GRID_PTR[*I_PTR].isFood) {
        GRID_PTR[*I_PTR].isFood = 0;

        for (;;) {
            *I_PTR = rand() % (GRID_X * GRID_Y);
            if (!GRID_PTR[*I_PTR].isWall && !GRID_PTR[*I_PTR].isSnake) {
                GRID_PTR[*I_PTR].isFood = 1;
                break;
            }
        }
    } else {
        *I_PTR = SNAKE_BODY_PTR[0];
        GRID_PTR[*I_PTR].isSnake = 0;

        for (*I_PTR = 1; *I_PTR < *SNAKE_LENGTH_PTR; (*I_PTR)++) {
            SNAKE_BODY_PTR[*I_PTR - 1] = SNAKE_BODY_PTR[*I_PTR];
        }
        (*SNAKE_LENGTH_PTR)--;
    }
}

int main(void) {
    SDL_Init(SDL_INIT_VIDEO | SDL_INIT_EVENTS);
    g = (uint8_t *) malloc(
        1024 * 1024
    );

    InitGrid();
    SDL_CreateWindowAndRenderer("grnsnake", SCREEN_WIDTH, SCREEN_HEIGHT, 0, WINDOW_PTR, RENDERER_PTR);

    *DX_PTR = 0;
    *DY_PTR = -1;

    while (1) {
        while (SDL_PollEvent(EVENT_PTR)) {
            switch (EVENT_PTR->type) {
                case SDL_EVENT_QUIT:
                    return 1;
                case SDL_EVENT_KEY_DOWN:
                    switch (EVENT_PTR->key.scancode) {
                        case SDL_SCANCODE_W: *DX_PTR = 0;
                            *DY_PTR = -1;
                            break;
                        case SDL_SCANCODE_S: *DX_PTR = 0;
                            *DY_PTR = 1;
                            break;
                        case SDL_SCANCODE_A: *DX_PTR = -1;
                            *DY_PTR = 0;
                            break;
                        case SDL_SCANCODE_D: *DX_PTR = 1;
                            *DY_PTR = 0;
                            break;
                    }
                default: break;
            }
        }

        MoveSnake();
        DrawGrid();
        SDL_Delay(100);
    }
}
```
