Pilote d'affichage TFT ST7789 pour STM32
Description
Cette bibliothèque fournit un pilote complet pour le contrôleur d'affichage TFT ST7789, optimisé pour les microcontrôleurs STM32 utilisant SPI HAL avec support DMA. Elle inclut toutes les fonctions de dessin de base, le rendu de texte avec plusieurs polices et des capacités d'affichage d'images.

Fonctionnalités principales :
Dessin accéléré matériellement avec DMA

Support de multiples rotations d'écran (0°, 90°, 180°, 270°)

Rendu en couleur 16 bits (RGB565)

Formes géométriques de base (lignes, rectangles, cercles, triangles)

Rendu de texte avec polices et tailles personnalisables

Affichage d'images (formats RGB565 et RGB888)

Utilitaires de conversion de couleur (RGB888 vers RGB565, HSV vers RGB565)

Fonctions de gestion d'alimentation (mode veille, contrôle de luminosité)

Exemples d'utilisation
1. Initialisation
c
// Définir vos broches GPIO et le handle SPI
GPIO_TypeDef* DC_GPIO_PORT = GPIOA;
uint16_t DC_PIN = GPIO_PIN_0;
GPIO_TypeDef* CS_GPIO_PORT = GPIOA;
uint16_t CS_PIN = GPIO_PIN_1;
GPIO_TypeDef* RST_GPIO_PORT = GPIOA;
uint16_t RST_PIN = GPIO_PIN_2;
SPI_HandleTypeDef hspi1;  // Votre handle SPI

// Initialiser l'affichage
ST7789_Init(DC_GPIO_PORT, DC_PIN, 
            CS_GPIO_PORT, CS_PIN, 
            RST_GPIO_PORT, RST_PIN, 
            &hspi1, 
            ST7789_ROTATION_90);
2. Dessin de base
c
// Remplir l'écran avec une couleur
ST7789_FillScreen(ST7789_BLUE);

// Dessiner un pixel rouge à (100, 50)
ST7789_DrawPixel(100, 50, ST7789_RED);

// Dessiner une ligne verte de (0,0) à (100,100)
ST7789_DrawLine(0, 0, 100, 100, ST7789_GREEN);

// Dessiner un rectangle blanc vide à (50,50) avec largeur 100 et hauteur 80
ST7789_DrawRect(50, 50, 100, 80, ST7789_WHITE);

// Dessiner un rectangle jaune plein à (150,50) avec largeur 50 et hauteur 50
ST7789_FillRect(150, 50, 50, 50, ST7789_YELLOW);

// Dessiner un cercle cyan à (120,160) avec rayon 30
ST7789_DrawCircle(120, 160, 30, ST7789_CYAN);
3. Affichage de texte
c
// Utiliser la police par défaut (vous devez définir votre police)
FontDef myFont = Font_7x10;  // Exemple de définition de police

// Dessiner du texte en blanc sur fond noir
ST7789_DrawStringEx(10, 10, "Bonjour !", ST7789_WHITE, ST7789_BLACK, myFont, 1);

// Dessiner du texte plus grand (taille 2x)
ST7789_DrawStringEx(10, 30, "Gros texte", ST7789_RED, ST7789_BLACK, myFont, 2);

// Dessiner du texte avec différentes couleurs et fond
ST7789_DrawStringEx(10, 60, "Coloré", ST7789_YELLOW, ST7789_MAGENTA, myFont, 1);
4. Affichage d'images
c
// Supposons que vous ayez une image au format RGB565 (240x320)
extern const uint16_t monImage[240*320];  // Vos données d'image

// Afficher l'image complète
ST7789_DrawImageDMA(0, 0, 240, 320, monImage);

// Afficher une partie de l'image
ST7789_DrawImageDMA(50, 50, 100, 100, &monImage[50*240 + 50]);
5. Fonctionnalités avancées
c
// Inverser les couleurs de l'affichage
ST7789_InvertColors(true);
HAL_Delay(1000);
ST7789_InvertColors(false);

// Régler la luminosité (si supporté)
ST7789_SetBrightness(75);  // 75% de luminosité

// Mettre l'affichage en veille
ST7789_Sleep(true);
HAL_Delay(2000);
ST7789_Sleep(false);

// Changer la rotation
ST7789_SetRotation(ST7789_ROTATION_180);
6. Conversion de couleurs
c
// Convertir RGB888 en RGB565
uint16_t couleurPerso = RGB888_to_RGB565(255, 128, 0);  // Orange
ST7789_FillRect(0, 0, 50, 50, couleurPerso);

// Créer une couleur à partir de valeurs HSV (Teinte: 0-360, Saturation: 0-1, Valeur: 0-1)
uint16_t couleurArcEnCiel = HSV_to_RGB565(120.0f, 1.0f, 1.0f);  // Vert pur
ST7789_FillRect(50, 0, 50, 50, couleurArcEnCiel);
Conseils de performance
Utilisez les versions DMA des fonctions pour de meilleures performances

Minimisez le nombre de changements de fenêtre d'adresse lors du dessin

Pour des écrans statiques, envisagez de tout dessiner dans un buffer avant l'envoi

Utilisez les fonctions de gestion d'alimentation quand l'affichage n'est pas nécessaire

Notes
La bibliothèque suppose que le SPI est configuré en mode 8 bits

La taille du buffer DMA peut être ajustée dans le fichier d'en-tête

Les définitions de police doivent être fournies séparément dans un fichier font.h
