- /*Alunos:Maria Helena
Dauinny Lilian
Raul Silva

Batalha naval é um jogo de tabuleiro de dois jogadores, no qual os jogadores têm de "adivinhar" em que quadrados estão os navios do oponente, através de palpites. Embora o primeiro jogo em tabuleiro comercializado e publicado pela Milton Bradley Company em 1931, o jogo foi originalmente jogado com lápis e papel.*/






#include <iostream>
#include <ctime>
#include <cstdlib>

using namespace std;

const int lin = 5, col = 5, slin = 3, scol = 2;

void iniciaTab(int tab[][col]);
void mostraTab(int tab[][col]);
void iniciaNav(int nav[][scol]);
void efetuaTiro(int tiro[]);
bool verificaTiro(int tiro[], int nav[][scol]);
void ajuda(int tiro[], int nav[][scol], int tent);

int main() {
    int tab[lin][col];
    int nav[slin][scol];
    int tiro[2];
    int tent = 0, exite = 0; // Changed "exit" to "exit" to avoid conflicts with the exit() function

    int op = 0;

    iniciaTab(tab);
    iniciaNav(nav);

    do {
        cout << "1-reiniciar jogo\n2-jogar\n3-dica\n4-mostrar tabuleiro\n5-sair";
        cout << "\nOpção: ";
        cin >> op;

        switch (op) {
            case 1:
                cout << "Reiniciando o jogo...\n";
                iniciaTab(tab);
                iniciaNav(nav);
                mostraTab(tab);
                break;

            case 2:
                do {
                    mostraTab(tab);
                    efetuaTiro(tiro);
                    tent++;

                    if (verificaTiro(tiro, nav)) {
                        cout << "Acertou!" << endl;
                        exite++;
                        tab[tiro[0]][tiro[1]] = 1;
                    }

                    else{

                        cout << endl << "Errou" << endl;
                        cout<<"Se quiser continuar tentando digite 2"<<endl;  
                      cout<<"caso contrario digite 5 para sair\n"<<endl;
                        tab[tiro[0]][tiro[1]] = 0;
                        if(tent>3){
                          int c=0;
                          cout<<"(1)dica..."<<endl;
                          cout<<"(2)continuar..."<<endl;
                          cin>>c;
                          if(c==1){ajuda(tiro, nav, tent);}else cout << endl;
                        }
                    }
                } while (exite != 3 && tent < 12); // Changed "exit" to "exit" and added "&& tent < 3"

                cout << "Você acertou " << exite << " navios" << endl;
                cout << "Você fez " << tent << " tentativas\n\n" << endl;

                if(exite==3){
                  int a=0;
                  cout << endl<< "Parabens vc ganhou!"<< endl;
                  cout << "Quer Jogar Novamente?" << endl;
                  cout << "(1)sim"<< endl << "(2)não" << endl;
                  cout << "...:";
                  cin >> a;
                  if(a==1){
                  exite=0;
                  iniciaTab(tab);
                  iniciaNav(nav);
                  mostraTab(tab);
                  }else if(a==2){exit(0);}else{
                    cout << "digite novamente:";
                    cin>>a;
                  }
                }
                break;

            case 3:

                ajuda(tiro, nav, tent);
                break;

            case 4:
                mostraTab(tab);
                break;

            case 5:
                return 0;
                break;

            default:
                break;
        }
    } while (op != 5);

    return 0;
}

void iniciaTab(int tab[][col]) {
    for (int i = 0; i < lin; i++) {
        for (int j = 0; j < col; j++) {
            tab[i][j] = -1;
        }
    }
}

void mostraTab(int tab[][col]) {
    cout << endl;
    cout << "\t\t\t\tTABULEIRO\t\t\t" << endl;
    cout<< "\tMar:~"<<endl;
  cout<< "\tErrou o tiro:*"<<endl;
  cout<< "\tAcertou o tiro:X\n"<<endl;

    for (int i = 0; i < lin; i++) {
        for (int j = 0; j < col; j++) {
            if (tab[i][j] == -1)
                cout << "\t~\t";
            else if (tab[i][j] == 0)
                cout << "\t*\t";
            else
                cout << "\tX\t";
        }
        cout << endl;
    }
    cout << endl;
}

void iniciaNav(int nav[][scol]) {
    srand(time(NULL));

    for (int i = 0; i < slin; i++) {
        nav[i][0] = rand() % lin;
        nav[i][1] = rand() % col;

        for (int j = 0; j < i; j++) {
            while (nav[i][0] == nav[j][0] && nav[i][1] == nav[j][1]) {
                nav[i][0] = rand() % lin;
                nav[i][1] = rand() % col;
            }
        }
    }


}

void efetuaTiro(int tiro[]) {
    cout << "\n\tEntre com as coordenadas!!!!\t\t\n";
    cout << "\tLinha(1-5): ";
    cin >> tiro[0];
    tiro[0]--;
    cout << "\tColuna(1-5): ";
    cin >> tiro[1];
    tiro[1]--;
}

bool verificaTiro(int tiro[], int nav[][scol]) {
    for (int i = 0; i < slin; i++) {
        if (tiro[0] == nav[i][0] && tiro[1] == nav[i][1]) {
            return true;
        }
    }
    return false;
}

void ajuda(int tiro[], int nav[][scol], int tent) {
    int column = 0, row = 0;

    for (int i = 0; i < slin; i++) {
        if (tiro[0] == nav[i][0]) {
            row++;
        }
        if (tiro[1] == nav[i][1]) {
            column++;
        }
    }
    cout << "------------------------"; 
    cout << endl;
    cout << "Aqui está a dica: " << endl;
    //cout << "\nDica: " << tent;
    cout << "Linha: " << tiro[0] + 1 << "; " << row << " Navios" << endl;
    cout << "Coluna: " << tiro[1] + 1 << "; " << column << " Navios" << endl <<"------------------------" <<endl;
}
